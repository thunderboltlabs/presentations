# RubyMotion for Faster Client/Server Development

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

!SLIDE title

# RubyMotion 

for Faster Client/Server Development

!SLIDE main-section

# Getting Started

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

## MacRuby: Quick Syntax

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  ...
end
```

...is the method (selector) 

#### `mapView:regionDidChangeAnimated:`

!SLIDE

## MacRuby: Quick Syntax

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  ...
end
```

...is the same as defining

``` objective_c
-(void)mapView:(MKMapView *)mapView regionDidChangeAnimated:(BOOL)animated
{
  ...
}
```

!SLIDE

## MacRuby: Quick Syntax

``` ruby
def mapView(map_view, regionDidChangeAnimated:animated)
  ...
end
```

...and is **not**

``` ruby
def mapView(map_view, opts = {})
  animated = opts[:regionDidChangeAnimated]
  ...
end
```

!SLIDE

## MacRuby: Quick Syntax

This means

``` ruby
mapView(map_view, regionDidChangeAnimated:animated)
```

and

``` ruby
mapView(map_view, regionWillChangeAnimated:animated)
```

are different methods.

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

}}}images/storyboard_tag.jpg

!SLIDE

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
end
```

!SLIDE

or...

### ib gem

``` ruby
class LoginViewController < UIViewController
  extend IB

  outlet :email_text_field
  outlet :name_text_field
  outlet :button
end
```

```
$ rake design
```

!SLIDE

# Bundler

``` ruby
# Rakefile
$:.unshift("/Library/RubyMotion/lib")
require 'motion/project'
require 'bundler'
Bundler.require
...
```

##### http://thunderboltlabs.com/posts/using-bundler-with-rubymotion

!SLIDE

# Cocoapods

``` ruby
# Gemfile
gem "motion-cocoapods"
```

``` ruby
Motion::Project::App.setup do |app|
  app.pods do
    pod "RestKit"
    pod "cocos2d"
  end
```

### https://github.com/CocoaPods/Specs
## Over 450 Pods and Counting

!SLIDE main-section

# iOS Patterns in Ruby

!SLIDE tiny

## Memory Matters&trade;

#### MapView

``` ruby
class MapViewController < UIViewController
  ...
  def mapView(mapView, viewForAnnotation: annotation)
    if view = mapView.dequeueReusableAnnotationViewWithIdentifier("ViewIdentifier")
      view.annotation = annotation
    else
      view = MKPinAnnotationView.alloc.initWithAnnotation(annotation,
                                                          reuseIdentifier:"ViewIdentifier")
    end
    return view
  end
end
```

!SLIDE

## Delegation

##### TableViewController

``` ruby
class MyTableController < UITableViewController
  def viewDidLoad
    view.dataSource = self # I'm the delegate for data
    view.delegate   = self # I'm the delegate for rendering
  end

  def tableView(tableView, numberOfRowsInSection:section)
    ...
  end

  def tableView(tableView, cellForRowAtIndexPath:indexPath)
    ...
  end
end
```

!SLIDE

## Callbacks

#### Animation

``` ruby
UIView.animateWithDuration(5.0 
                           animations: lambda {
                             ...
                           }
                           completion: lambda{|finished|
                             ...
                           })

```

!SLIDE main-section

# Client / Server

!SLIDE

# AFNetworking

![AFNetworking](images/afnetworking.png)

##### Gowalla | https://github.com/AFNetworking/AFNetworking

!SLIDE

# AFNetworking

#### Streaming
#### Progress monitoring
#### Authentication
#### Canceling/suspension/resuming
#### Caching
#### Success/failures though HTTP codes

!SLIDE

# AFNetworking

``` ruby
url = NSURL.URLWithString("https://url.com")
request = NSURLRequest.requestWithURL(url)
operation = AFJSONRequestOperation.JSONRequestOperationWithRequest(
              request, 
              success:lambda {|request, response, json|
                ...
              }, 
              failure:nil)
operation.start
```

!SLIDE

# AFNetworking

### Built for Subclassing

``` ruby
MyClient.sharedClient.getPath("/foo",
                              parameters: nil,
                              success: lambda { ... },
                              failure: lambda { ... })
```

!SLIDE

# SDWebImage

#### Caching, asynchronous image downloader

##### Olivier Poitrey | https://github.com/rs/SDWebImage

!SLIDE

# What not to do...

``` ruby
url  = NSURL.URLWithString("http://path.to/image.jpg")

data = NSData.dataWithContentsOfURL(url)

img  = UIImage.alloc.initWithData(data)

image_view.setImage(img)
```

## Why?

!SLIDE

}}} images/beachball.jpeg

!SLIDE

# SDWebImage

##### Simply install the CocoaPod, and...

``` ruby
url = NSURL.URLWithString("http://path.to/image.jpg")

placeholder = UIImage.imageNamed("placeholder.png")

image_view.setImageWithURL(url, placeholderImage: placeholder)
```

!SLIDE

# RestKit

## HTTP+JSON based ORM

##### GateGuru | https://github.com/RestKit/RestKit

!SLIDE

### Warning:

## RestKit 0.20 is eminent!

##### Follow @restkit for announcements

!SLIDE left

## Easy as...

1. Write a Plain Old Ruby model
1. Tell RestKit what JSON to map
1. GET a URL

#### Returns your models like magic

!SLIDE

### Write a PORO model

``` ruby
class Person
  attr_accessor :remote_id, :name
end
```

!SLIDE

### Tell RestKit what JSON to map

``` json
{
  "people": [
    { "id": 1, "name": "John Ritter"  },
    { "id": 2, "name": "Suzanne Somers"  },
    { "id": 3, "name": "Joyce DeWitt"  },
    { "id": 4, "name": "Don Knotts"  },
    ...
  ]
}
```

!SLIDE

### Tell RestKit what JSON to map

!SLIDE

``` ruby
class AppDelegate
  attr_accessor :window, :backend

  def application(application, didFinishLaunchingWithOptions:launchOptions)
    url = NSURL.URLWithString("http://backend.dev")
    self.backend = RKObjectManager.managerWithBaseURL(url)
    add_mapping(person_mapping, "person")
    add_mapping(person_mapping, "people")
    true
  end

  def person_mapping
    @person_mapping ||= begin
      mapping = RKObjectMapping.mappingForClass(Person)
      mapping.addAttributeMappingsFromDictionary(id: "remote_id",
                                                 name: "name")
    end
  end

  def add_mapping(mapping, path)
    success = RKStatusCodeIndexSetForClass(RKStatusCodeClassSuccessful)
    descriptor = RKResponseDescriptor.responseDescriptorWithMapping(mapping,
                                        pathPattern: nil,
                                        keyPath: path,
                                        statusCodes: success)
    backend.addResponseDescriptor(descriptor)
  end
end
```

!SLIDE

### Get an arbitrary URL

``` ruby
def viewDidAppear(animated)
  App.delegate.backend.getObjectsAtPath("/people",
    parameters: nil,
    success: lambda { |operation, result| ... },
    failure: lambda { |operation, error|  ... })
end
```

!SLIDE

### Returns your models like magic

``` ruby
App.delegate.backend.getObjectsAtPath("/people",
    parameters: nil,
    success: lambda do |operation, result| 
      result.array       # Array of Person objects
      result.firstObject # First Person object
    end,
    failure: ...)
```

!SLIDE

# Full CRUD

!SLIDE

##### Serialize Request Objects to JSON

``` ruby
class AppDelegate
  def application(application, didFinishLaunchingWithOptions:launchOptions)
    ...
    add_request_mapping(person_mapping, "person")
  end

  def person_mapping
    @person_mapping ||= begin
      mapping = RKObjectMapping.mappingForClass(Person)
      mapping.addAttributeMappingsFromDictionary(id: "remote_id",
                                                 name: "name")
    end
  end

  def add_request_mapping(mapping, path)
    klass = mapping.objectClass
    descriptor = RKRequestDescriptor.requestDescriptorWithMapping(
                   mapping.inverseMapping,
                   objectClass: klass,
                   rootKeyPath: path)
    backend.addRequestDescriptor(descriptor)
  end
end
```

!SLIDE

##### Add Routes

``` ruby
get_route    = RKRoute.routeWithClass(Person,
                                      pathPattern: "/people/:remote_id",
                                      method: RKRequestMethodGET)
put_route    = RKRoute.routeWithClass(Person,
                                      pathPattern: "/people/:remote_id",
                                      method: RKRequestMethodPUT)
delete_route = RKRoute.routeWithClass(Person,
                                      pathPattern: "/people/:remote_id",
                                      method: RKRequestMethodDELETE)
post_route   = RKRoute.routeWithClass(Person,
                                      pathPattern: "/people",
                                      method: RKRequestMethodPOST)

backend.router.routeSet.addRoute(get_route)
backend.router.routeSet.addRoute(put_route)
backend.router.routeSet.addRoute(delete_route)
backend.router.routeSet.addRoute(post_route)
```

!SLIDE

##### Create

``` ruby
App.delegate.backend.postObject(@person,
                                path:nil,
                                parameters:nil,
                                success: ...,
                                failure: ...)
```

##### POST /people with `person[id]`, `person[name]`

!SLIDE

##### Update

``` ruby
App.delegate.backend.putObject(@person,
                               path:nil,
                               parameters:nil,
                               success: ...,
                               failure: ...)
```

##### POST /people/3 with `person[id]`, `person[name]`

##### RestKit transforms "/people/:remote_id" to "/people/3"

!SLIDE main-section

# DEMO! 

## OMGWAT???

!SLIDE

# Beyond

Object relationships, Core Data, Caching, Image uploading and more.

!SLIDE

The big question...

# Is it worth it?

!SLIDE main-section

## The Pain

!SLIDE

## Say goodbye to Ruby Gems

#### No #require or #eval

### You wouldn't want them, anyways

!SLIDE

# Strange bugs

!SLIDE left trollface

#### wrong number of arguments (-1339225320 for 0)

### No method name
### No line number
### No backtrace

!SLIDE left trollface

#### wrong number of arguments (-1339225320 for 0)

### No method name
### No line number
### No backtrace

# Fixed

!SLIDE

## ...but it's moving *fast*

!SLIDE

### Lack of deep debugging tools

#### They're working on this.

!SLIDE center

# Cocoa is Huge

}}} images/huge_library.jpg

!SLIDE

# Closed Source

![closed](images/closed.jpg)

!SLIDE

### ...but backed by an amazing team...

# Laurent Sansonetti

![closed](images/lrz.jpeg)

!SLIDE apple_evil

!SLIDE main-section

# The Glory!

!SLIDE

# Expressiveness of Ruby

### (rather, "The Ugliness of Obj-C")

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

# No more XCode 

!SLIDE center

}}} images/text_from_xcode.png

!SLIDE

}}} images/emacs.png

!SLIDE center

# (just kidding)

}}} images/emacs.png

!SLIDE bottom-left

}}} images/vim.jpg

# Code in Vim and drive with Rake

!SLIDE center

# Same Language as Backend

}}} images/chinese_dancers.jpg

!SLIDE

# Ethos of the Ruby Community

}}} images/indiana_jones.jpg

!SLIDE center

# Production vs. Internal?

}}} images/sand_castle.jpg

!SLIDE main-section

## TODO: Remember to plug the book, dumbass!

!SLIDE logo

# Questions?

#### @thunderboltlabs
#### http://thunderboltlabs.com

