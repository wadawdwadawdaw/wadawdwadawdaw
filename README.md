import 'package:flutter/material.dart';
import 'package:flutter_blue/flutter_blue.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Smart Pen'),
        ),
        body: SmartPenPage(),
      ),
    );
  }
}

class SmartPenPage extends StatefulWidget {
  @override
  _SmartPenPageState createState() => _SmartPenPageState();
}

class _SmartPenPageState extends State<SmartPenPage> {
  FlutterBlue flutterBlue = FlutterBlue.instance;
  BluetoothDevice? device;

  @override
  void initState() {
    super.initState();
    scanForDevices();
  }

  void scanForDevices() {
    flutterBlue.scan().listen((scanResult) {
      if (scanResult.device.name == 'SmartPen') {
        setState(() {
          device = scanResult.device;
        });
        flutterBlue.stopScan();
      }
    });
  }

  void connectToDevice() async {
    if (device != null) {
      await device!.connect();
      // Discover services and characteristics
      List<BluetoothService> services = await device!.discoverServices();
      // Implement logic to read data from the pen
    }
  }

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Text('Smart Pen App'),
          SizedBox(height: 20),
          ElevatedButton(
            onPressed: connectToDevice,
            child: Text('Connect to Smart Pen'),
          ),
        ],
      ),
    );
  }
}
