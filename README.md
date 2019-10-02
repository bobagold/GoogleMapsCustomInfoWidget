Example Implementation:

```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

import 'custom_info_widget.dart';

void main() => runApp(MyApp());

class PointObject {
  final Widget child;
  final LatLng location;

  PointObject({this.child, this.location});
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      initialRoute: "/",
      routes: {
        "/": (context) => HomePage(),
      },
    );
  }
}

class HomePage extends StatelessWidget {
  final PointObject point = PointObject(
    child: Text('Lorem Ipsum'),
    location: LatLng(47.6, 8.8796),
  );

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        color: Colors.green,
        child: GoogleMap(
          initialCameraPosition: CameraPosition(
            target: const LatLng(47.6, 8.6796),
            zoom: 10,
          ),
          markers: Set<Marker>()
            ..add(Marker(
              markerId: MarkerId(point.location.toString()),
              position: point.location,
              onTap: () => _onTap(context, point),
            )),
        ),
      ),
    );
  }

  void _onTap(BuildContext context, PointObject point) {
    final RenderObject renderObject = context.findRenderObject();
    if (renderObject is! RenderBox) {
      return;
    }
    final RenderBox renderBox = renderObject as RenderBox;
    Rect _itemRect = renderBox.localToGlobal(Offset.zero) & renderBox.size;

    InfoWidgetRoute _infoWidgetRoute = InfoWidgetRoute(
      child: point.child,
      buildContext: context,
      textStyle: const TextStyle(
        fontSize: 14,
        color: Colors.black,
      ),
      mapsWidgetSize: _itemRect,
      height: 50,
    );

    Navigator.of(context, rootNavigator: true).push<dynamic>(_infoWidgetRoute);
  }
}
```
