# HeapProfiler

TODO: Describe your gem

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'heap-profiler'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install heap-profiler

## Usage

### Profiling Mode

HeapProfiler can be used to mesure memory allocations and retentions of a Ruby code snippet.

To record a profile:

```ruby
require 'heap-profiler'
HeapProfiler.report('path/to/report/directory') do
  # You code here
end
```

To then analyse the profile

```bash
$ heap-profiler path/to/report/directory
Total allocated: 4.12 kB (36 objects)
Total retained: 624.00 B (10 objects)

allocated memory by gem
-----------------------------------
   3.56 kB  heap-profiler/bin
  336.00 B  other
  232.00 B  heap-profiler/lib

allocated memory by file
-----------------------------------
   3.56 kB  heap-profiler/bin/generate-report
  232.00 B  heap-profiler/lib/heap_profiler/reporter.rb

allocated memory by location
-----------------------------------
   3.09 kB  heap-profiler/bin/generate-report:29
  232.00 B  heap-profiler/lib/heap_profiler/reporter.rb:89
  157.00 B  heap-profiler/bin/generate-report:23
   72.00 B  heap-profiler/bin/generate-report:17
   40.00 B  heap-profiler/bin/generate-report:26
   40.00 B  heap-profiler/bin/generate-report:25
   40.00 B  heap-profiler/bin/generate-report:21
   40.00 B  heap-profiler/bin/generate-report:20
   40.00 B  heap-profiler/bin/generate-report:19
   40.00 B  heap-profiler/bin/generate-report:18

allocated memory by class
-----------------------------------
   1.18 kB  Class
  848.00 B  <iseq> (IMEMO)
  597.00 B  String
  528.00 B  Hash
  384.00 B  <ment> (IMEMO)
  232.00 B  File
  112.00 B  Array
   80.00 B  <ifunc> (IMEMO)
   80.00 B  <cref> (IMEMO)
   40.00 B  Symbol
   40.00 B  SomeCustomStuff

allocated objects by gem
-----------------------------------
        33  heap-profiler/bin
         2  other
         1  heap-profiler/lib

allocated objects by file
-----------------------------------
        33  heap-profiler/bin/generate-report
         1  heap-profiler/lib/heap_profiler/reporter.rb

allocated objects by location
-----------------------------------
        25  heap-profiler/bin/generate-report:29
         1  heap-profiler/lib/heap_profiler/reporter.rb:89
         1  heap-profiler/bin/generate-report:26
         1  heap-profiler/bin/generate-report:25
         1  heap-profiler/bin/generate-report:23
         1  heap-profiler/bin/generate-report:21
         1  heap-profiler/bin/generate-report:20
         1  heap-profiler/bin/generate-report:19
         1  heap-profiler/bin/generate-report:18
         1  heap-profiler/bin/generate-report:17

allocated objects by class
-----------------------------------
        12  String
         8  <ment> (IMEMO)
         3  Hash
         2  Class
         2  Array
         2  <iseq> (IMEMO)
         2  <ifunc> (IMEMO)
         2  <cref> (IMEMO)
         1  Symbol
         1  SomeCustomStuff
         1  File

retained memory by gem
-----------------------------------
  392.00 B  heap-profiler/bin
  232.00 B  heap-profiler/lib

retained memory by file
-----------------------------------
  392.00 B  heap-profiler/bin/generate-report
  232.00 B  heap-profiler/lib/heap_profiler/reporter.rb

retained memory by location
-----------------------------------
  232.00 B  heap-profiler/lib/heap_profiler/reporter.rb:89
  160.00 B  heap-profiler/bin/generate-report:29
   72.00 B  heap-profiler/bin/generate-report:17
   40.00 B  heap-profiler/bin/generate-report:21
   40.00 B  heap-profiler/bin/generate-report:20
   40.00 B  heap-profiler/bin/generate-report:19
   40.00 B  heap-profiler/bin/generate-report:18

retained memory by class
-----------------------------------
  240.00 B  String
  232.00 B  File
   72.00 B  Array
   40.00 B  Symbol
   40.00 B  SomeCustomStuff

retained objects by gem
-----------------------------------
         9  heap-profiler/bin
         1  heap-profiler/lib

retained objects by file
-----------------------------------
         9  heap-profiler/bin/generate-report
         1  heap-profiler/lib/heap_profiler/reporter.rb

retained objects by location
-----------------------------------
         4  heap-profiler/bin/generate-report:29
         1  heap-profiler/lib/heap_profiler/reporter.rb:89
         1  heap-profiler/bin/generate-report:21
         1  heap-profiler/bin/generate-report:20
         1  heap-profiler/bin/generate-report:19
         1  heap-profiler/bin/generate-report:18
         1  heap-profiler/bin/generate-report:17

retained objects by class
-----------------------------------
         6  String
         1  Symbol
         1  SomeCustomStuff
         1  File
         1  Array

Allocated String Report
-----------------------------------
   80.00 B       2  "foo="
                 2  heap-profiler/bin/generate-report:29

   80.00 B       2  "foo"
                 2  heap-profiler/bin/generate-report:29

   80.00 B       2  "bar="
                 2  heap-profiler/bin/generate-report:29

   80.00 B       2  "I am retained"
                 1  heap-profiler/bin/generate-report:19
                 1  heap-profiler/bin/generate-report:18

   40.00 B       1  "I am retained too"
                 1  heap-profiler/bin/generate-report:20

   40.00 B       1  "I am allocated too"
                 1  heap-profiler/bin/generate-report:26

   40.00 B       1  "I am allocated"
                 1  heap-profiler/bin/generate-report:25

  157.00 B       1  "I am a very very long string I am a very very long string I am a very very long string I am a very very long string "
                 1  heap-profiler/bin/generate-report:23


Retained String Report
-----------------------------------
   80.00 B       2  "I am retained"
                 1  heap-profiler/bin/generate-report:19
                 1  heap-profiler/bin/generate-report:18

   40.00 B       1  "foo="
                 1  heap-profiler/bin/generate-report:29

   40.00 B       1  "foo"
                 1  heap-profiler/bin/generate-report:29

   40.00 B       1  "bar="
                 1  heap-profiler/bin/generate-report:29

   40.00 B       1  "I am retained too"
                 1  heap-profiler/bin/generate-report:20
```


### Heap Analysis

Alternatively if you with to analyse the entire heap of your Ruby process.

If you can, you should enable allocation tracing as early as possible during your application boot process, e.g. in `config/boot.rb` for Rails apps.

```ruby
require 'objspace'
ObjectSpace.trace_object_allocations_start.
```

Then to dump the heap:

```ruby
require 'objspace'
ObjectSpace.dump_all(output: File.open('path/to/file.heap', 'w+'))
```

Then run `heap-profiler` against it:

```bash
heap-profiler path/to/file.heap
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/Shopify/heap-profiler.


## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
