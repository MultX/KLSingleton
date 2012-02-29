## Description

This is a singleton class for iOS, Objective-C, Cocoa Touch, iPhone, iPad. We offer no judgement as to whether or not to use singletons. If you do use singletons in your project, then use NSSingleton. NSSingleton is named in a manner designed to minimize the amount of discussion about singletons, pro or con.

NSSingleton is:

1. Subclassible (to the n-th degree)
2. ARC compatible
3. Thread-safe (uses +initialize, not @synchronize)
4. Macro-free
5. Swizzle-free
6. Simple

It has been tested only with ARC. This implementation chooses "not lazy" instead of "requires explicit initialization" or "locks to allocate".

## Use

1.  Add the files to your project
2.  Import the header using  `#import "NSSingleton.h"`
3.  Subclass the NSSingleton class in the following way:

<pre>
@interface MYSubclass : NSSingleton
</pre>

You may then retrieve the unique, ready-to-use instance of your class by calling any of the following methods:

<pre>
[MYSubclass instance]
[MYSubclass sharedInstance] //alias
[MYSubclass singleton]      //alias
[[MYSubclass alloc] init]   //bad style, but safe to call any number of times
</pre>

## Future Directions

1. This has been tested on ARC only. Test on projects without ARC support.
2. See if there are any meaningful differences with the swizzle method.
3. Macro the options to become lazy and either "require initialization" or "lock to allocate"
4. Something like `dispatch_once` that ran once for each subclass would let us be lazy for free

## License

Released under ISC (similar to 2-clause BSD)

## Testing / Verification

<pre>
@interface NSAlpha : NSSingleton
@end
@interface NSBravo : NSSingleton
@end
@interface NSCharlie : NSBravo
@end

@implementation NSAlpha
@end
@implementation NSBravo
@end
@implementation NSCharlie
@end

id a,b,c,d;
NSLog(@"%d=%d=%d", a=[NSSingleton instance], [[NSSingleton alloc] init], [[NSSingleton alloc] init]);
NSLog(@"%d=%d=%d", b=[NSAlpha instance],     [[NSAlpha alloc] init],     [[NSAlpha alloc] init]);
NSLog(@"%d=%d=%d", c=[NSBravo instance],     [[NSBravo alloc] init],     [[NSBravo alloc] init]);
NSLog(@"%d=%d=%d", d=[NSCharlie instance],   [[NSCharlie alloc] init],   [[NSCharlie alloc] init]);
NSLog(@"%d != %d != %d != %d", a, b, c, d);
</pre>
