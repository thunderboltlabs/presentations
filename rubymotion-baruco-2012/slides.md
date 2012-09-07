# RubyMotion in Practice

!SLIDE intro center

# RubyMotion in Practice

}}}images/rubymotion.png

!SLIDE logo

# Thunderbolt Labs

#### Tammer Saleh & Randall Thomas

!SLIDE

# Please interrupt us.

#### (We get bored up here)

!SLIDE

# We're pretty damn good at a lot of stuff...

Ruby, C++, Whisk(e)y, UNIX, TDD, ADD, Pool, Rails, Git, Earthquakes, Guns, Spec, Drinking, Neural Nets, Math, Statistics (51% of the time), Data, Agile, Clojure, SOAs, Site downtime, REST

!SLIDE intro

# But we are _not_ iOS experts

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


!SLIDE center

}}} images/text_from_xcode.png

!SLIDE

# No more XCode 

#### (sorta)

!SLIDE

}}} images/emacs.png

!SLIDE center

# (just kidding)

}}} images/emacs.png

!SLIDE bottom-left

}}} images/vim.jpg

# Code in Vim and drive with Rake

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
class AppDelegate
  def application(application, didFinishLaunchingWithOptions:launchOptions)
    true
  end
end
```

``` shell
$ rake
     Build ./build/iPhoneSimulator-5.1-Development
     Build vendor/TestFlight
     Build vendor/Pods
      Link ./build/iPhoneSimulator-5.1-Development/ArtInMyCoffee.app/ArtInMyCoffee
     ...
```

!SLIDE

## MacRuby

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  # ...
end
```

is the method (selector): **mapView:regionDidChangeAnimated:**

!SLIDE

## MacRuby

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  # ...
end
```

``` objective_c
-(void)mapView:(MKMapView *)mapView regionDidChangeAnimated:(BOOL)animated
{
  //...
}
```

!SLIDE

## MacRuby

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  # ...
end
```

is **not**:

``` ruby
def mapView(map_view, opts = {})
  animated = opts[:regionDidChangeAnimated]
  # ...
end
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

## Configure

``` shell
$ ls resources/
MyApp.storyboard
```

``` ruby
# Rakefile
Motion::Project::App.setup do |app|
  app.info_plist["UIMainStoryboardFile"] = "MyApp"
```

!SLIDE bottom-left

## Use

}}}images/storyboard_tag.jpg

!SLIDE

## Use

``` ruby
class LoginViewController < UIViewController
  def email_text_field
    @email_text_field ||= view.viewWithTag(1)
  end

  def password_text_field
    @password_text_field ||= view.viewWithTag(2)
  end

  def button
    @button ||= view.viewWithTag(3)
  end
```

!SLIDE

# Cocoapods

gem "motion-cocoapods"

``` ruby
Motion::Project::App.setup do |app|
  app.pods do
    pod "RestKit"
    pod "cocos2d"
  end
```

### https://github.com/CocoaPods/Specs
## Over 450 Pods and Counting

!SLIDE

# iOS Patterns in Ruby

!SLIDE

# MapView

``` ruby
class MapViewController < UIViewController
  def viewDidLoad
    # load annotations using map.addAnnotation(object)
  end

  def map
    @map ||= view.viewWithTag(1)
  end

  def mapView(mapView, viewForAnnotation: annotation)
    if view = mapView.dequeueReusableAnnotationViewWithIdentifier("ViewIdentifier")
      view.annotation = annotation
    else
      view = MKPinAnnotationView.alloc.initWithAnnotation(annotation, 
                                                          reuseIdentifier:"ViewIdentifier")
      ...
    end
    return view 
  end
end
```

!SLIDE

#### TableViewController

``` ruby
class CoffeeShopSelectController < UITableViewController
  def viewDidLoad
    view.dataSource = self # I'm the delegate for data
    view.delegate   = self # I'm the delegate for rendering
    @coffee_shops = [...]
  end

  def tableView(tableView, numberOfRowsInSection:section)
    @coffee_shops.size
  end

  def tableView(tableView, cellForRowAtIndexPath:indexPath)
    cell = tableView.dequeueReusableCellWithIdentifier("CoffeeShopCell") || begin
      CoffeeShopCell.alloc.initWithStyle(UITableViewCellStyleDefault, 
                                         reuseIdentifier:"CoffeeShopCell")
    end

    cell.coffee_shop = @coffee_shops[indexPath.row]
    return cell
  end
end
```

!SLIDE

## Instance reuse

``` ruby
def tableView(tableView, cellForRowAtIndexPath:indexPath)
  # Try to grab an old instance and reuse it...
  cell = tableView.dequeueReusableCellWithIdentifier("CoffeeShopCell") || begin
    # Only create a new one if we have to...
    CoffeeShopCell.alloc.initWithStyle(UITableViewCellStyleDefault, 
                                       reuseIdentifier:"CoffeeShopCell")
  end

  cell.coffee_shop = @coffee_shops[indexPath.row]
  return cell
end
```

!SLIDE

## Delegation

``` ruby
class CoffeeShopSelectController < UITableViewController
  def viewDidLoad
    view.delegate   = self # I'm the main delegate for rendering
    view.dataSource = self # I'm the delegate for data
  end

  def tableView(tableView, numberOfRowsInSection:section)
    # sent to dataSource delegate
    ...
  end

  def tableView(tableView, cellForRowAtIndexPath:indexPath)
    # sent to main delegate
    ...
  end
end
```

!SLIDE

# RestKit

## Maps JSON keys to your models

!SLIDE left

1. Write a PORO model
1. Tell RestKit what JSON to map
1. Get an arbitrary URL
1. Returns your models like magic

#### http://bit.ly/rubymotion-restkit

!SLIDE

### Write a PORO model

``` ruby
class CoffeeShop
  attr_accessor :id,
                :latitude,
                :longitude,
                :name,
                :shot_count,
                :created_at,
                :updated_at
end
```

!SLIDE

### Tell RestKit what JSON to map

``` json
{
  "coffee_shops": [
    {
      "id": 1,
      "name": "Blue Bottle",
      "latitude": "20.0",
      "longitude": "-122.4075",
      "shot_count": 4
    },
    ...
  ]
}
```

!SLIDE

### Tell RestKit what JSON to map

``` ruby
def coffee_shop_mapping
  mapping = RKObjectMapping.mappingForClass(CoffeeShop)
  mapping.mapKeyPath("id",         toAttribute: "id")
  mapping.mapKeyPath("latitude",   toAttribute: "latitude")
  mapping.mapKeyPath("longitude",  toAttribute: "longitude")
  mapping.mapKeyPath("name",       toAttribute: "name")
  mapping.mapKeyPath("shot_count", toAttribute: "shot_count")
  mapping.mapKeyPath("created_at", toAttribute: "created_at")
  mapping.mapKeyPath("updated_at", toAttribute: "updated_at")
  return mapping
end

restkit_object_manager.mappingProvider.setMapping(coffee_shop_mapping, 
                                                  forKeyPath:"coffee_shops")
```

!SLIDE

### Get an arbitrary URL

``` ruby
class CoffeeShopSelectController < UITableViewController
  def viewDidLoad
    object_manager = RKObjectManager.objectManagerWithBaseURLString(base_url)
    object_manager.delegate = self
    object_manager.get("/coffee_shops", self)
  end
```

!SLIDE

### Returns your models like magic

``` ruby
class CoffeeShopSelectController < UITableViewController
  ...
  def objectLoader(object_loader, didLoadObjects:coffee_shops)
    add_coffee_shops(coffee_shops)
  end

  def objectLoader(object_loader, didFailWithError:error)
    log "Error: #{error.inspect}"
  end
```

!SLIDE

The big question...

# Is it worth it?

!SLIDE main-section

## The Pain

!SLIDE

# Say goodbye to Ruby Gems

### No #require, #send, #eval...

!SLIDE

## "useless" memoization to work around ARC bugs

``` ruby
def email_text_field
  @email_text_field ||= view.viewWithTag(1)
end
```

!SLIDE

# Strange bugs

!SLIDE left trollface

```
wrong number of arguments (-1339225320 for 0)
```

#### No method name
#### No line number
#### No backtrace

!SLIDE

``` ruby
(#<UIRoundedRectButton:0xa194dd...)> MyApp(15862,0xacac8a28) malloc:
*** error for object 0xa19f2f4: 
  incorrect checksum for freed object - 
  object was probably modified after being freed.
*** set a breakpoint in malloc_error_break to debug
```

!SLIDE

### ...And just today...

``` shell
$ rake
     Build ./build/iPhoneSimulator-5.1-Development
     Build vendor/TestFlight
     Build vendor/Pods
      Link ./build/iPhoneSimulator-5.1-Development/ArtInMyCoffee.app/ArtInMyCoffee
Undefined symbols for architecture i386:
  "_NSDeletedObjectsKey", referenced from:
      -[RKForm reloadObjectOnContextDidSaveNotification:] in libPods.a(RKForm.o)
      ... 40 lines of crap...
      _OBJC_METACLASS_$_RKSearchWord in libPods.a(RKSearchWord.o)
ld: symbol(s) not found for architecture i386
clang: error: linker command failed with exit code 1 (use -v to see invocation)
rake aborted!
Command failed with status (1): [/Applications/Xcode.app/Contents/Developer...]
Tasks: TOP => default => simulator => build:simulator
(See full trace by running task with --trace)
```

!SLIDE

## Accessing views in storyboards

``` ruby
def email_text_field
  @email_text_field ||= view.viewWithTag(1)
end
```

### Manual and brittle

##### "ib" gem can help

!SLIDE

# Lack of deep debugging tools

!SLIDE

# TDD on RubyMotion suuuucks

!SLIDE center

# Cocoa is Huge

}}} images/huge_library.jpg

!SLIDE

# Closed Source

![closed](images/closed.jpg)

!SLIDE apple_evil

!SLIDE center

# Production vs. Hobby

}}} images/sand_castle.jpg

!SLIDE logo

# Questions?

#### @thunderboltlabs
#### http://thunderboltlabs.com
