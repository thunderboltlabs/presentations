# RubyMotion in Practice

!SLIDE intro

# h1
## h2
### h3
#### h4
##### h5
###### h6

!SLIDE intro

# RubyMotion in Practice

}}}images/rubymotion.png

!SLIDE intro

## The Why
## The Beauty
## The Pain

!SLIDE main-section

# The Why

!SLIDE

# Expressiveness of Ruby

### Rather: The Ugliness of Obj-C

``` objective_c
[[[Foo alloc] initWithBar:bar andBaz:baz] autorelease]

[self performSelector:@selector(mytest:withAString:) withObject: mystring];
```

!SLIDE tiny

``` objective_c
- (UITableViewCell *)tableView:(UITableView *)tableView 
                               cellForRowAtIndexPath:(NSIndexPath *)indexPath 
{
  UITableViewCell *cell = [self.tableView dequeueReusableCellWithIdentifier:@"MyIdentifier"];
  if (cell == nil) 
  {
    cell = [[[UITableViewCell alloc] initWithFrame:CGRectZero 
                                     reuseIdentifier:@"MyIdentifier"] 
            autorelease];
  }
  NSDictionary *itemAtIndex = (NSDictionary *)[menuList objectAtIndex:indexPath.row];
  cell.text = [itemAtIndex objectForKey:@"option"];
  return cell;
}
```


!SLIDE

}}} images/text_from_xcode.png

!SLIDE

# No more XCode 

#### (sorta)

## Code in Vim and drive with Rake

!SLIDE bottom-left

# Ethos of the Ruby Community

}}} images/indiana_jones.jpg

!SLIDE main-section

# The Code

!SLIDE

## Getting Started

!SLIDE left

``` shell
$ motion create sample
    Create sample
    Create sample/.gitignore
    Create sample/Rakefile
    Create sample/app
    Create sample/app/app_delegate.rb
    Create sample/resources
    Create sample/spec
    Create sample/spec/main_spec.rb
```

``` ruby
# app/app_delegate.rb 
class AppDelegate
  def application(application, didFinishLaunchingWithOptions:launchOptions)
    true
  end
end
```

!SLIDE

## MacRuby

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  # ...
end
```

...is a single selector, and gets compiled into:

``` objective_c
-(void)mapView:(MKMapView *)mapView regionDidChangeAnimated:(BOOL)animated
{
  //...
}
```

!SLIDE

## MacRuby

This means

``` ruby
mapView(map_view, regionDidChangeAnimated:animated)
```

and

``` ruby
mapView(map_view, regionWillChangeAnimated:animated)
```

...are different methods.

!SLIDE bottom-left

# Storyboards

}}}images/storyboard.jpg

!SLIDE

#### RestKit

!SLIDE

### iOS Patterns in Ruby

!SLIDE

#### MapView

!SLIDE

#### TableViewController

!SLIDE main-section

## The Pain

!SLIDE

# Strange bugs

!SLIDE left

```
wrong number of arguments (-1339225320 for 0)
```

#### No method name
#### No line number
#### No backtrace

!SLIDE

#### Strange GC issues

``` ruby
(#<UIRoundedRectButton:0xa194dd...)> MyApp(15862,0xacac8a28) malloc:
*** error for object 0xa19f2f4: 
  incorrect checksum for freed object - 
  object was probably modified after being freed.
*** set a breakpoint in malloc_error_break to debug
```

!SLIDE

### Cargo Culting

!SLIDE

#### "useless" memoization to work around ARC bugs

!SLIDE

#### RestKit Mapper calls

!SLIDE

### Swimming Up Stream

!SLIDE

#### Translating obj-c to ruby

!SLIDE

#### Accessing views in storyboards

!SLIDE

