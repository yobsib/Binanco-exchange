# Binanco-exchange
Binanco exchange app for exchange cryptocurrency features 
import 'package:firebase_auth/firebase_auth.dart';
import 'package:google_sign_in/google_sign_in.dart';

final GoogleSignIn _googleSignIn = GoogleSignIn();
final FirebaseAuth _auth = FirebaseAuth.instance;

Future<User?> signInWithGoogle() async {
  final GoogleSignInAccount? googleUser = await _googleSignIn.signIn();
  final GoogleSignInAuthentication googleAuth = await googleUser!.authentication;
  
  final AuthCredential credential = GoogleAuthProvider.credential(
    accessToken: googleAuth.accessToken,
    idToken: googleAuth.idToken,
  );
  
  final UserCredential userCredential = await _auth.signInWithCredential(credential);
  return userCredential.user;
}ElevatedButton(
  onPressed: () {
    print("Referral Program");
  },
  child: Text('Start Referral Program'),
),ElevatedButton(
  onPressed: () {
    print("P2P exchange started!");
  },
  child: Text('Start P2P Exchange'),
),import 'package:http/http.dart' as http;
import 'dart:convert';

Future<void> fetchMarketData() async {
  final response = await http.get(Uri.parse('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd'));

  if (response.statusCode == 200) {
    var data = json.decode(response.body);
    print(data); // Handle the market data here
  } else {
    throw Exception('Failed to load market data');
  }
}import 'package:cloud_firestore/cloud_firestore.dart';

Future<void> storeKYCData(String userId, Map<String, dynamic> kycData) async {
  await FirebaseFirestore.instance.collection('kyc').doc(userId).set(kycData);
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Binanco Exchange',
      theme: ThemeData(
        primaryColor: Colors.yellow, // Yellow theme
        scaffoldBackgroundColor: Colors.black, // Black background
        textTheme: TextTheme(
          bodyText1: TextStyle(color: Colors.white), // White text
          bodyText2: TextStyle(color: Colors.white),
        ),
        buttonTheme: ButtonThemeData(buttonColor: Colors.yellow), // Button Color Yellow
        appBarTheme: AppBarTheme(color: Colors.black), // AppBar Black
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Binanco Exchange"),
      ),
      body: Center(
        child: Text(
          'Welcome to Binanco Exchange!',
          style: TextStyle(color: Colors.white),
        ),
      ),
    );
  }
}class FakeDeposit {
  double balance = 0.0;

  // Fake deposit function
  void fakeDeposit(double amount) {
    balance += amount;
    print('Fake Deposit Successful! Your new balance is: \$${balance}');
  }
}

void main() {
  FakeDeposit user = FakeDeposit();
  user.fakeDeposit(1000.0); // Adding fake money
}class User {
  bool isKYCComplete = false; // By default, KYC is not complete
  double balance = 0.0;

  // Function to complete KYC
  void completeKYC() {
    isKYCComplete = true;
    print('KYC Completed');
  }

  // Withdrawal function with KYC check
  void withdraw(double amount) {
    if (!isKYCComplete) {
      print('You cannot withdraw until you complete KYC!');
      return;
    }
    if (balance >= amount) {
      balance -= amount;
      print('Withdrawal Successful! New balance: \$${balance}');
    } else {
      print('Insufficient funds.');
    }
  }

  // Function to deposit fake money
  void fakeDeposit(double amount) {
    balance += amount;
    print('Fake Deposit Successful! Your balance: \$${balance}');
  }
}

void main() {
  User user = User();
  
  // Fake deposit of 1000
  user.fakeDeposit(1000.0);
  
  // Try to withdraw without completing KYC
  user.withdraw(500.0); // This should fail due to KYC not being completed
  
  // Complete KYC and try withdrawing again
  user.completeKYC();
  user.withdraw(500.0); // This should succeed now
}
