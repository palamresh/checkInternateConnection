# checkInternateConnection


import 'dart:async';

import 'package:connectivity_plus/connectivity_plus.dart';
import 'package:demoexample/screen/first_screen.dart';
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:internet_connection_checker/internet_connection_checker.dart';

class CheckInternet extends StatefulWidget {
  const CheckInternet({super.key});

  @override
  State<CheckInternet> createState() => _CheckInternetState();
}

class _CheckInternetState extends State<CheckInternet> {
  StreamSubscription<List<ConnectivityResult>>? subscription;

  bool isInternetConnected = true;

  @override
  void initState() {
    super.initState();
    subscription = Connectivity()
        .onConnectivityChanged
        .listen((List<ConnectivityResult> result) async {
      final checker = InternetConnectionChecker
          .instance; // Ensure instance is properly created
      bool isConnected = await checker.hasConnection;
      setState(() {
        isInternetConnected = isConnected;
      });
    });
  }

  @override
  void dispose() {
    subscription?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: isInternetConnected ? Colors.blue : Colors.green,
      appBar: AppBar(
        title: const Text("Check Internet"),
      ),
      body: isInternetConnected
          ? Center(
              child: ElevatedButton(
                  onPressed: () {
                    Navigator.push(context,
                        MaterialPageRoute(builder: (context) => FirstScreen()));
                  },
                  child: Text("Go to the next page")))
          : Center(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.center,
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Icon(
                    Icons.signal_wifi_off,
                    size: 100,
                    color: Colors.red,
                  ),
                  SizedBox(height: 20),
                ],
              ),
            ),
    );
  }
}
