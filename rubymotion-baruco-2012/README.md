# RubyMotion in Practice

## The Good

### Expressiveness of Ruby

### No more XCode (sorta)

### Ethos of the Ruby Community

[Picture of Indiana Jones]

## Examples

### Simple app

#### Storyboards

#### RestKit

### iOS Patterns in Ruby

#### MapView

#### TableViewController

## The Bad

### Strange bugs

#### Airity bug

#### Strange GC issues

{: lang=ruby}
    (#<UIRoundedRectButton:0xa194dd...)> MyApp(15862,0xacac8a28) malloc:
    *** error for object 0xa19f2f4: 
      incorrect checksum for freed object - 
      object was probably modified after being freed.
    *** set a breakpoint in malloc_error_break to debug

### Cargo Culting

#### "useless" memoization to work around ARC bugs

#### RestKit Mapper calls

### Swimming Up Stream

#### Translating obj-c to ruby

#### Using storyboards

