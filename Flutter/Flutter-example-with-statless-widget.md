```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyWidget());
}

class MyWidget extends StatelessWidget {
  const MyWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: const Center(
            child: Text("DeadWatch"),
          ),
          backgroundColor: const Color.fromRGBO(122, 162, 227, 1),
        ),
        body: const Center(child: Text("hello")),
        backgroundColor: const Color.fromRGBO(246, 241, 241, 1),
      ),
    );
  }
}
```