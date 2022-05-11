# Custom touch handling

Most native components for iOS apps are supported out of the box by our remote control feature. Occasionally you may find a component that requires custom touch handling does not work with our remote control feature. To support these use cases, we provide a way to tap into the events coming from Cobrowse to implement custom touch handling in your app. A view or gesture recogniser can access the touch events via the following callback methods.

```objectivec
-(void) cobrowseTouchesBegan:(NSSet<CBIOTouch*> *)touches withEvent:(CBIOTouchEvent*)event;
-(void) cobrowseTouchesMoved:(NSSet<CBIOTouch*> *)touches withEvent:(CBIOTouchEvent*)event;
-(void) cobrowseTouchesEnded:(NSSet<CBIOTouch*> *)touches withEvent:(CBIOTouchEvent*)event;
```

Custom key events may be handled via a separate callback.

```objectivec
-(void) cobrowseKeyDown:(CBIOKeyPress*)event;
```
