---
layout: post
title: Creating a gem with bundler
date: 2013-11-26 10:48:31
disqus: n
---

A trick I am learning since 2 years ago : splitting up even more of the main code base into smaller parts. The ideas behind this are numerous : from speeding up tests to keeping things simple.

Defining sub parts of the product as gems allows to enforce stricter rules on the APIs changes, keep each test suite small and independent and stick to principles and rules dear to OO programmers and OO designers.

And in the same manner rather than using Rspec and tons of gems to test drive the development of each gem I rely on something that’s already coming with Ruby : Minitest.

## A great companion for the journey

After a quick search I found a great Minitest quick reference from Matt Sears (http://mattsears.com/articles/2011/12/10/minitest-quick-reference). It has been very helpful to jump on the minitest train with my Rspec habits. I only had to look for something else when I started to mock objects and messages.

## Starting a gem

Bundler has the great taste to come with a way to create a gem skeleton : ```bundle gem```.

And if you ask bundle a bit of details about that ```gem``` command :

```
bundle help gem
Usage:
  bundle gem GEM

Options:
  -b, [--bin=Generate a binary for your library.]
  -t, [--test=Generate a test directory for your library: 'rspec' is the default, but 'minitest' is also supported.]
  -e, [--edit=/path/to/your/editor]  # Open generated gemspec in the specified editor (defaults to $EDITOR or $BUNDLER_EDITOR)
      [--no-color=Disable colorization in output]
  -V, [--verbose=Enable verbose output mode]

Creates a skeleton for creating a rubygem
```

Let’s run it :

```
$ bundle gem foo -t minitest
      create  foo/Gemfile
      create  foo/Rakefile
      create  foo/LICENSE.txt
      create  foo/README.md
      create  foo/.gitignore
      create  foo/foo.gemspec
      create  foo/lib/foo.rb
      create  foo/lib/foo/version.rb
      create  foo/test/minitest_helper.rb
      create  foo/test/test_foo.rb
      create  foo/.travis.yml
Initializating git repo in /Users/ange/Code/gems/test/foo
```

The command creates a *foo* directory and create everything you need to start working on a new gem :

* a *Gemfile* (yet it’s mostly empty)
* a *Rakefile*
* a *LICENSE* (MIT license, by default it considers you are going open source)
* a basic *README* with minimal sections
* a *.gitignore* file already filed with usual files and paths
* a *foo.gemspec* file that contains the description of your gem, the gem development and running dependencies and your contact informations
* then there is the initial, almost empty, files for your gem placed and named following the classic practices of the Ruby community, including *minitest* base setup.
* as a bonus there is a minimal *.travis.yml* config file so you can push your code to a public repository on github and get the tests runnings right away !

## Gemspec

Contrary to a Ruby application (such as a Sinatra or Rails app) you don’t define your gem dependencies in a Gemfile. You define them in the gem spec file :

```
# coding: utf-8
lib = File.expand_path('../lib', __FILE__)
$LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
require 'foo/version'

Gem::Specification.new do |spec|
  spec.name          = "foo"
  spec.version       = Foo::VERSION
  spec.authors       = ["Thomas Riboulet"]
  spec.email         = ["riboulet+gem@som.thing”]
  spec.description   = %q{TODO: Write a gem description}
  spec.summary       = %q{TODO: Write a gem summary}
  spec.homepage      = ""
  spec.license       = "MIT"

  spec.files         = `git ls-files`.split($/)
  spec.executables   = spec.files.grep(%r{^bin/}) { |f| File.basename(f) }
  spec.test_files    = spec.files.grep(%r{^(test|spec|features)/})
  spec.require_paths = ["lib"]

  spec.add_development_dependency "bundler", "~> 1.3"
  spec.add_development_dependency "rake"
  spec.add_development_dependency "minitest"

  spec.add_dependency “faraday”
end
```

This gemspec  is the one generated by bundler. We can see that it includes the gem name, the version (using the Foo::VERSION constant inherited from the ```require ‘foo/version’``` line. It also includes my name, email, the description and summary of the gem, a homepage and license.
Bundler will use git tricks to list the files, and some ruby tricks to list executables and test files.

The dependencies come last with first the development dependencies and then the dependencies. The first ones will be needed to develop, the later just to use the gem.

## Minitest

Now I do like to have a default test setup ready but this one lacks colours and readability to my taste.
I did not know about the -t flag until I did some research for this post. I was happy to see it in but I do prefer my setup and way :

```
# test helper
require 'minitest/autorun'
require 'minitest/pride'

$LOAD_PATH.unshift File.expand_path('../../lib', __FILE__)
require 'foo'
```

Pride will add a bit of colours to the output. (Look around there is also a way to have a nyan cat running across the screen).

The Rake file is ok but here is an alternative one :

```
# Rakefile
require "bundler/gem_tasks"

desc "Run all tests"
task :test do
  Dir.glob('./test/**/test_*.rb').each { |file| require file }
end

task :default => :test
```

It’s a bit simpler to read and there is less libs required. It’s using a very simple mechanism to run all tests : *Dir.glob* call on the test directory plus a *require* for each test file.

## Test file template

As you can see from the previous Rakefile and the initial run of bundler the naming rule for minitest test files is to *prefix* the name of the tested class with *test_*.

I am not going to give a crash course on Minitest : all you have to know is described in the **excellent** article from Matt Sears.

What I can show you is a basic template.

```
require_relative ‘../../minitest_helper'

describe Foo do
  describe "setup" do
     it “should respond to setup” do
        subject.must_respond_to(:setup)
     end
  end
end
```

The first line might look odd to some : I usually keep the Rails’ habit to sort my tests the same way the tested classes are. It keeps thing a bit nittier all around.

```
Foo/
   lib/
     foo.rb
     foo/
        sub_foo.rb
   test/
       minitest_helper.rb
       lib/
          test_foo.rb
          foo/
             test_sub_foo.rb
```

That’s why the require relative is a playing with paths.

## Other gems I use with Minitest

### VCR

For those who don’t know VCR it’s a nifty tool that allows you to record http transactions with external services and replay them. It allows you to speed up test and also to avoid calling 100 times a minute a remote service. Bonus point : you can work offline if you have a set of recordings.

VCR uses *cassettes* to record and replay. Cassettes are YAML files that you can edit yourself if you ever want to. There is a simple setup to do in your test helper :

```
VCR.configure do |c|
  c.default_cassette_options = { :record => :new_episodes }
  c.cassette_library_dir = 'test/vcr/cassettes'
  c.hook_into :faraday
end
```

You can setup default cassette options such as the preference to record (never, every time, once, …), where to store the cassettes and what library to use to pass the http calls and replay them.
For the options the simple way is to request *new_episodes*. That will record new requests and store them to be replayed when they are requested again. It’s what you would usually want.
For the cassette dir, again I like my directories organised so I put all the vcr stuff inside the test directory.
For the library I usually go for Faraday. It’s the http lib I use most of the time now : it’s simple, allows for some clean code and fast tricks while doing the job properly. It’s also pretty simple to configure.

Back to the test template : the before and after blocks setup and cleanup the VCR run. ```VCR.insert_cassette __name__``` will create a cassette named after the current test file and tell VCR to write to it.
```VCR.eject_cassette``` does what you might imagine : close the file to avoid further writes.

```
    before do
      VCR.insert_cassette __name__
    end

    after do
      VCR.eject_cassette
    end
```

The problem with VCR is that once it’s setup all your http calls will go through it and it’s a bit tedious to setup paths around it to avoid that.

> An alternative to using record and replay tools such as VCR is to use tools such as Artifice (https://github.com/wycats/artifice).
> Artifice allows you to mock remote service replies by plugging http calls to mini Rack apps.
> It’s a bit more difficult to setup and maintain than VCR but it’s way faster and if you have a proper documentation of the remote API or recordings of how it’s behaving it can be a nicer solution.

### Faker

Faker is another really handy tool : it allows you to generate realistic data for you model attributes. Names, email addresses, phone numbers, domain names, … It’s a handy companion for people writing tests everyday.

### Fake-redis

I often use redis for all sort of things in my applications and gems. Fake redis is a memory only, super fast implementation of Redis. It will save you time and make tests faster.

### Mocha

If Minitest is not enough Mocha can bring a bit more bang to it, it’s also easy to use and will ease your path to mocks.

## Packaging up the gem

Once it’s ready you can build the gem using ```gem build```. Then you can push to Rubygems, if it’s to be open source that it is.
Create an account on Rubygems.org if it’s not the case yet and check the documentation over there. Once it’s done you can ```gem push foo-0.0.1.gem``` to get your gem out there.

## Using your gems privately

There is commercial private gem servers as a service such as Gemfury (http://www.gemfury.com) but you can use private git repositories to store your private gems too.

## The point

I hope that this article has shown you how easy it has become to create a gem with tests. I am quite sad I did not know about these steps few years ago, it would have made it easier for me to keep my projects tighter and cleaner.

### Thanks

Big thanks to @joelazemar for his advices when I was searching around and learning about gem making.

Big thanks to Matt Sears for his great article about Minitest.

Big thanks to Claudio Ortolina for his brilliant piece (http://net.tutsplus.com/tutorials/ruby/gem-creation-with-bundler/) that served as inspiration for this article. If you need more details go check it out. The present article is intended to be a quick read to give you the minimal things you need to get going but Claudio’s has lot of details.

And a huge thank you to the Bundler team for the tool and commands it brings.
