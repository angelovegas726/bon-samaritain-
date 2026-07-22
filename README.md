name: bon_samaritain
description: Application d'urgence communautaire Bon Samaritain.
publish_to: "none"

version: 1.0.0+1

environment:
  sdk: ">=3.5.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.8

  # Firebase
  firebase_core: ^3.15.1
  firebase_auth: ^5.3.3
  cloud_firestore: ^5.4.5
  firebase_messaging: ^15.1.5
  firebase_storage: ^12.3.4
  firebase_analytics: ^11.3.5
  firebase_crashlytics: ^4.1.5

  # Carte et GPS
  google_maps_flutter: ^2.9.0
  geolocator: ^13.0.2
  geocoding: ^3.0.0

  # État de l'application
  flutter_riverpod: ^2.6.1

  # Navigation
  go_router: ^14.6.2

  # Notifications locales
  flutter_local_notifications: ^18.0.1

  # Permissions
  permission_handler: ^11.3.1

  # Caméra et médias
  camera: ^0.11.0+2
  image_picker: ^1.1.2

  # Réseau
  connectivity_plus: ^6.1.0

  # Stockage local
  shared_preferences: ^2.3.2

  # Utilitaires
  intl: ^0.19.0
  uuid: ^4.5.1
  url_launcher: ^6.3.1

  # Monétisation
  google_mobile_ads: ^5.2.0
  in_app_purchase: ^3.2.0

dev_dependencies:
  flutter_test:
    sdk: flutter

  flutter_lints: ^5.0.0

flutter:
  uses-material-design: true

  assets:
    - assets/images/
    - assets/icons/
    - assets/sounds/
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

import 'app.dart';

Future<void> main() async {
WidgetsFlutterBinding.ensureInitialized();

await Firebase.initializeApp();

runApp(const BonSamaritainApp());
}
import 'package:flutter/material.dart';

import 'core/theme/app_theme.dart';
import 'features/home/home_page.dart';

class BonSamaritainApp extends StatelessWidget {
  const BonSamaritainApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Bon Samaritain',
      debugShowCheckedModeBanner: false,

      theme: AppTheme.light,

      darkTheme: AppTheme.dark,

      themeMode: ThemeMode.dark,

      home: const HomePage(),
    );
  }
}
import 'package:flutter/material.dart';

class AppTheme {
  static const Color gold = Color(0xFFD4AF37);

  static ThemeData dark = ThemeData(
    brightness: Brightness.dark,

    scaffoldBackgroundColor: const Color(0xFF0F0F0F),

    primaryColor: gold,

    colorScheme: ColorScheme.fromSeed(
      seedColor: gold,
      brightness: Brightness.dark,
    ),

    appBarTheme: const AppBarTheme(
      backgroundColor: Colors.black,
      centerTitle: true,
      foregroundColor: gold,
    ),

    floatingActionButtonTheme:
        const FloatingActionButtonThemeData(
      backgroundColor: gold,
      foregroundColor: Colors.black,
    ),

    elevatedButtonTheme:
        ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        backgroundColor: gold,
        foregroundColor: Colors.black,
        shape: RoundedRectangleBorder(
          borderRadius:
              BorderRadius.circular(16),
        ),
      ),
    ),
  );

  static ThemeData light = dark;
}
import 'package:flutter/material.dart';

class HomePage extends StatelessWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("🛡 Bon Samaritain"),
      ),
      body: SafeArea(
        child: Padding(
          padding: const EdgeInsets.all(20),
          child: Column(
            children: [

              const SizedBox(height: 20),

              const Text(
                "Votre sécurité, notre mission",
                style: TextStyle(
                  fontSize: 18,
                  color: Colors.white70,
                ),
              ),

              const Spacer(),

              SizedBox(
                width: 220,
                height: 220,
                child: ElevatedButton(
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.red,
                    shape: const CircleBorder(),
                  ),
                  onPressed: () {
                    // TODO : Ouvrir l'écran SOS
                  },
                  child: const Text(
                    "SOS",
                    style: TextStyle(
                      fontSize: 42,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),

              const SizedBox(height: 40),

              GridView.count(
                crossAxisCount: 2,
                shrinkWrap: true,
                mainAxisSpacing: 12,
                crossAxisSpacing: 12,
                childAspectRatio: 2.8,
                children: [

                  _actionCard(
                    Icons.favorite,
                    "Urgence médicale",
                    Colors.red,
                  ),

                  _actionCard(
                    Icons.local_police,
                    "Agression",
                    Colors.orange,
                  ),

                  _actionCard(
                    Icons.car_crash,
                    "Accident",
                    Colors.blue,
                  ),

                  _actionCard(
                    Icons.local_fire_department,
                    "Incendie",
                    Colors.deepOrange,
                  ),
                ],
              ),

              const Spacer(),

            ],
          ),
        ),
      ),

      bottomNavigationBar: BottomNavigationBar(
        type: BottomNavigationBarType.fixed,

        items: const [

          BottomNavigationBarItem(
            icon: Icon(Icons.home),
            label: "Accueil",
          ),

          BottomNavigationBarItem(
            icon: Icon(Icons.map),
            label: "Carte",
          ),

          BottomNavigationBarItem(
            icon: Icon(Icons.people),
            label: "Communauté",
          ),

          BottomNavigationBarItem(
            icon: Icon(Icons.history),
            label: "Historique",
          ),

          BottomNavigationBarItem(
            icon: Icon(Icons.settings),
            label: "Réglages",
          ),
        ],
      ),
    );
  }

  static Widget _actionCard(
      IconData icon,
      String title,
      Color color,
      ) {
    return Card(
      elevation: 5,
      child: InkWell(
        borderRadius: BorderRadius.circular(12),
        onTap: () {},

        child: Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [

            Icon(
              icon,
              color: color,
            ),

            const SizedBox(width: 8),

            Text(title),
          ],
        ),
      ),
    );
  }
}
import 'dart:async';

import 'package:flutter/material.dart';

class SosPage extends StatefulWidget {
  const SosPage({super.key});

  @override
  State<SosPage> createState() => _SosPageState();
}

class _SosPageState extends State<SosPage> {

  int seconds = 10;

  Timer? timer;

  bool alertSent = false;

  @override
  void initState() {
    super.initState();

    startCountdown();
  }

  void startCountdown() {

    timer = Timer.periodic(
      const Duration(seconds: 1),
      (timer) {

        if (seconds == 0) {

          timer.cancel();

          sendAlert();

        } else {

          setState(() {
            seconds--;
          });

        }
      },
    );
  }

  void sendAlert() {

    setState(() {
      alertSent = true;
    });

    // ----------------------------
    // A FAIRE

    // GPS

    // Firebase

    // Notification proches

    // Notification volontaires

    // Démarrage caméra

    // Démarrage micro

    // ----------------------------
  }

  @override
  void dispose() {

    timer?.cancel();

    super.dispose();

  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text("SOS"),
      ),

      body: Center(

        child: alertSent

            ? const Column(

                mainAxisAlignment: MainAxisAlignment.center,

                children: [

                  Icon(
                    Icons.check_circle,
                    color: Colors.green,
                    size: 120,
                  ),

                  SizedBox(height: 20),

                  Text(
                    "ALERTE ENVOYÉE",
                    style: TextStyle(
                      fontSize: 28,
                      fontWeight: FontWeight.bold,
                    ),
                  ),

                ],
              )

            : Column(

                mainAxisAlignment: MainAxisAlignment.center,

                children: [

                  const Text(

                    "Déclenchement dans",

                    style: TextStyle(
                      fontSize: 24,
                    ),

                  ),

                  const SizedBox(height: 20),

                  Text(

                    "$seconds",

                    style: const TextStyle(

                      fontSize: 90,

                      fontWeight: FontWeight.bold,

                      color: Colors.red,

                    ),

                  ),

                  const SizedBox(height: 40),

                  ElevatedButton.icon(

                    onPressed: () {

                      timer?.cancel();

                      Navigator.pop(context);

                    },

                    icon: const Icon(Icons.cancel),

                    label: const Text("ANNULER"),

                    style: ElevatedButton.styleFrom(

                      backgroundColor: Colors.red,

                      foregroundColor: Colors.white,

                      minimumSize: const Size(220, 60),

                    ),

                  ),

                ],

              ),

      ),

    );

  }

}
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:geolocator/geolocator.dart';
import 'package:uuid/uuid.dart';

class SosService {

  final FirebaseFirestore firestore =
      FirebaseFirestore.instance;

  final FirebaseAuth auth =
      FirebaseAuth.instance;

  final uuid = const Uuid();

  Future<void> sendSOS() async {

    // Vérifie que l'utilisateur est connecté
    final user = auth.currentUser;

    if (user == null) {
      throw Exception("Utilisateur non connecté");
    }

    // Vérifie que le GPS est activé
    bool serviceEnabled =
        await Geolocator.isLocationServiceEnabled();

    if (!serviceEnabled) {
      throw Exception("GPS désactivé");
    }

    // Vérifie les permissions
    LocationPermission permission =
        await Geolocator.checkPermission();

    if (permission == LocationPermission.denied) {

      permission =
          await Geolocator.requestPermission();

      if (permission == LocationPermission.denied) {
        throw Exception("Permission refusée");
      }
    }

    if (permission ==
        LocationPermission.deniedForever) {

      throw Exception(
        "Permission GPS refusée définitivement",
      );
    }

    // Position actuelle
    Position position =
        await Geolocator.getCurrentPosition(
      desiredAccuracy: LocationAccuracy.best,
    );

    // Création de l'alerte
    String alertId = uuid.v4();

    await firestore
        .collection("alerts")
        .doc(alertId)
        .set({

      "id": alertId,

      "userId": user.uid,

      "latitude": position.latitude,

      "longitude": position.longitude,

      "status": "ACTIVE",

      "type": "SOS",

      "createdAt": FieldValue.serverTimestamp(),

      "battery": null,

      "address": "",

      "volunteers": [],

      "contactsNotified": false,

      "volunteersNotified": false,
    });
  }
}
import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';

class NotificationService {

  static final FlutterLocalNotificationsPlugin
      localNotifications =
      FlutterLocalNotificationsPlugin();

  /// Initialisation
  static Future<void> initialize() async {

    const AndroidInitializationSettings
        androidSettings =
        AndroidInitializationSettings(
      '@mipmap/ic_launcher',
    );

    const InitializationSettings settings =
        InitializationSettings(
      android: androidSettings,
    );

    await localNotifications.initialize(
      settings,
    );

    await FirebaseMessaging.instance
        .requestPermission(
      alert: true,
      badge: true,
      sound: true,
    );
  }

  /// Notification locale
  static Future<void> showLocalNotification({
    required String title,
    required String body,
  }) async {

    const AndroidNotificationDetails
        androidDetails =
        AndroidNotificationDetails(
      "sos_channel",
      "SOS",
      channelDescription:
          "Notifications d'urgence",
      importance: Importance.max,
      priority: Priority.high,
    );

    const NotificationDetails details =
        NotificationDetails(
      android: androidDetails,
    );

    await localNotifications.show(
      0,
      title,
      body,
      details,
    );
  }

  /// Notification aux contacts d'urgence
  Future<void> notifyEmergencyContacts(
      String alertId) async {

    // TODO :
    // Lire les contacts dans Firestore
    // Envoyer une notification FCM
  }

  /// Notification aux volontaires proches
  Future<void> notifyNearbyVolunteers(
      double latitude,
      double longitude) async {

    // TODO :
    // Rechercher les volontaires
    // dans un rayon (500 m, 1 km...)
    // puis envoyer une notification.
  }

  /// Écoute des notifications reçues
  static void listen() {

    FirebaseMessaging.onMessage.listen(
      (RemoteMessage message) {

        showLocalNotification(
          title:
              message.notification?.title ??
              "Bon Samaritain",

          body:
              message.notification?.body ??
              "",
        );

      },
    );
  }
}
import 'dart:async';

import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:geolocator/geolocator.dart';

class LocationService {
  final FirebaseFirestore firestore =
      FirebaseFirestore.instance;

  final FirebaseAuth auth =
      FirebaseAuth.instance;

  StreamSubscription<Position>? _positionStream;

  /// Démarre le suivi GPS en temps réel
  Future<void> startLocationTracking() async {
    final user = auth.currentUser;

    if (user == null) return;

    bool serviceEnabled =
        await Geolocator.isLocationServiceEnabled();

    if (!serviceEnabled) return;

    LocationPermission permission =
        await Geolocator.checkPermission();

    if (permission == LocationPermission.denied) {
      permission =
          await Geolocator.requestPermission();
    }

    if (permission == LocationPermission.denied ||
        permission ==
            LocationPermission.deniedForever) {
      return;
    }

    _positionStream = Geolocator.getPositionStream(
      locationSettings: const LocationSettings(
        accuracy: LocationAccuracy.best,
        distanceFilter: 5,
      ),
    ).listen((Position position) async {
      await firestore
          .collection("users")
          .doc(user.uid)
          .update({
        "latitude": position.latitude,
        "longitude": position.longitude,
        "speed": position.speed,
        "heading": position.heading,
        "lastUpdate": FieldValue.serverTimestamp(),
      });
    });
  }

  /// Arrête le suivi GPS
  Future<void> stopLocationTracking() async {
    await _positionStream?.cancel();
    _positionStream = null;
  }

  /// Position actuelle
  Future<Position> getCurrentPosition() async {
    return Geolocator.getCurrentPosition(
      desiredAccuracy: LocationAccuracy.best,
    );
  }
}
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

class MapPage extends StatefulWidget {
  const MapPage({super.key});

  @override
  State<MapPage> createState() => _MapPageState();
}

class _MapPageState extends State<MapPage> {

  GoogleMapController? _controller;

  static const CameraPosition initialPosition =
      CameraPosition(
    target: LatLng(50.4820, 2.6400),
    zoom: 15,
  );

  final Set<Marker> _markers = {

    const Marker(
      markerId: MarkerId("me"),
      position: LatLng(50.4820,2.6400),
      infoWindow: InfoWindow(
        title: "Ma position",
      ),
    ),

    const Marker(
      markerId: MarkerId("hospital"),
      position: LatLng(50.4860,2.6430),
      icon: BitmapDescriptor.defaultMarkerWithHue(
        BitmapDescriptor.hueAzure,
      ),
      infoWindow: InfoWindow(
        title: "Hôpital",
      ),
    ),

    const Marker(
      markerId: MarkerId("police"),
      position: LatLng(50.4810,2.6370),
      icon: BitmapDescriptor.defaultMarkerWithHue(
        BitmapDescriptor.hueBlue,
      ),
      infoWindow: InfoWindow(
        title: "Police",
      ),
    ),

    const Marker(
      markerId: MarkerId("fire"),
      position: LatLng(50.4840,2.6385),
      icon: BitmapDescriptor.defaultMarkerWithHue(
        BitmapDescriptor.hueOrange,
      ),
      infoWindow: InfoWindow(
        title: "Pompiers",
      ),
    ),
  };

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text("Carte"),
      ),

      body: GoogleMap(

        initialCameraPosition:
            initialPosition,

        myLocationEnabled: true,

        myLocationButtonEnabled: true,

        compassEnabled: true,

        zoomControlsEnabled: false,

        markers: _markers,

        onMapCreated:
            (GoogleMapController controller) {

          _controller = controller;

        },

      ),

      floatingActionButton:

          FloatingActionButton.extended(

        onPressed: () {

          _controller?.animateCamera(

            CameraUpdate.newCameraPosition(
              initialPosition,
            ),

          );

        },

        icon: const Icon(Icons.my_location),

        label: const Text("Moi"),

      ),

    );

  }

}
import 'package:flutter/material.dart';

class ContactsPage extends StatefulWidget {
  const ContactsPage({super.key});

  @override
  State<ContactsPage> createState() => _ContactsPageState();
}

class _ContactsPageState extends State<ContactsPage> {

  final List<Map<String, dynamic>> contacts = [];

  void addContact() {

    showDialog(

      context: context,

      builder: (context) {

        final nameController =
            TextEditingController();

        final phoneController =
            TextEditingController();

        return AlertDialog(

          title: const Text(
            "Ajouter un contact",
          ),

          content: Column(

            mainAxisSize: MainAxisSize.min,

            children: [

              TextField(

                controller: nameController,

                decoration:
                    const InputDecoration(
                  labelText: "Nom",
                ),

              ),

              TextField(

                controller: phoneController,

                keyboardType:
                    TextInputType.phone,

                decoration:
                    const InputDecoration(
                  labelText: "Téléphone",
                ),

              ),

            ],

          ),

          actions: [

            TextButton(

              onPressed: () {

                Navigator.pop(context);

              },

              child: const Text("Annuler"),

            ),

            ElevatedButton(

              onPressed: () {

                setState(() {

                  contacts.add({

                    "name":
                        nameController.text,

                    "phone":
                        phoneController.text,

                  });

                });

                Navigator.pop(context);

              },

              child: const Text("Ajouter"),

            ),

          ],

        );

      },

    );

  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(

        title: const Text(
          "Contacts d'urgence",
        ),

      ),

      body: ListView.builder(

        itemCount: contacts.length,

        itemBuilder: (context, index) {

          return ListTile(

            leading:
                const Icon(Icons.person),

            title:
                Text(contacts[index]["name"]),

            subtitle:
                Text(contacts[index]["phone"]),

            trailing: IconButton(

              icon: const Icon(
                Icons.delete,
                color: Colors.red,
              ),

              onPressed: () {

                setState(() {

                  contacts.removeAt(index);

                });

              },

            ),

          );

        },

      ),

      floatingActionButton:
          FloatingActionButton(

        onPressed: addContact,

        child: const Icon(Icons.add),

      ),

    );

  }

}
import 'package:flutter/material.dart';

class VolunteersPage extends StatefulWidget {
  const VolunteersPage({super.key});

  @override
  State<VolunteersPage> createState() =>
      _VolunteersPageState();
}

class _VolunteersPageState
    extends State<VolunteersPage> {

  final List<Map<String, dynamic>> volunteers = [

    {
      "name": "Marc",
      "distance": "320 m",
      "skill": "PSC1",
      "available": true,
    },

    {
      "name": "Sophie",
      "distance": "1.2 km",
      "skill": "Infirmière",
      "available": true,
    },

    {
      "name": "Thomas",
      "distance": "2.8 km",
      "skill": "Pompier",
      "available": false,
    },
  ];

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text(
          "Communauté Bon Samaritain",
        ),
      ),

      body: ListView.builder(

        itemCount: volunteers.length,

        itemBuilder: (context, index) {

          final user = volunteers[index];

          return Card(

            margin: const EdgeInsets.all(10),

            child: ListTile(

              leading: CircleAvatar(

                backgroundColor:

                    user["available"]

                        ? Colors.green

                        : Colors.grey,

                child: const Icon(
                  Icons.person,
                ),

              ),

              title: Text(user["name"]),

              subtitle: Column(

                crossAxisAlignment:
                    CrossAxisAlignment.start,

                children: [

                  Text(user["skill"]),

                  Text(
                    "Distance : ${user["distance"]}",
                  ),

                ],

              ),

              trailing:

                  user["available"]

                      ? ElevatedButton(

                          onPressed: () {

                            // TODO

                            // Voir le profil

                          },

                          child: const Text(
                            "Voir",
                          ),

                        )

                      : const Text(
                          "Occupé",
                        ),

            ),

          );

        },

      ),

    );

  }

}

import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

class SettingsPage extends StatefulWidget {
  const SettingsPage({super.key});

  @override
  State<SettingsPage> createState() =>
      _SettingsPageState();
}

class _SettingsPageState
    extends State<SettingsPage> {

  bool gpsEnabled = true;
  bool cameraEnabled = true;
  bool microphoneEnabled = true;
  bool notificationsEnabled = true;
  bool batteryAlert = true;
  bool volunteerAlert = true;
  bool darkMode = true;

  @override
  void initState() {
    super.initState();
    loadSettings();
  }

  Future<void> loadSettings() async {

    final prefs =
        await SharedPreferences.getInstance();

    setState(() {

      gpsEnabled =
          prefs.getBool("gps") ?? true;

      cameraEnabled =
          prefs.getBool("camera") ?? true;

      microphoneEnabled =
          prefs.getBool("microphone") ?? true;

      notificationsEnabled =
          prefs.getBool("notifications") ?? true;

      batteryAlert =
          prefs.getBool("battery") ?? true;

      volunteerAlert =
          prefs.getBool("volunteers") ?? true;

      darkMode =
          prefs.getBool("dark") ?? true;

    });
  }

  Future<void> save(
      String key,
      bool value) async {

    final prefs =
        await SharedPreferences.getInstance();

    await prefs.setBool(key, value);
  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text(
          "Paramètres",
        ),
      ),

      body: ListView(

        children: [

          SwitchListTile(

            title:
                const Text("GPS"),

            subtitle:
                const Text(
              "Partager la position",
            ),

            value: gpsEnabled,

            onChanged: (value) {

              setState(() {
                gpsEnabled = value;
              });

              save("gps", value);

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Caméra",
            ),

            value: cameraEnabled,

            onChanged: (value) {

              setState(() {
                cameraEnabled = value;
              });

              save("camera", value);

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Microphone",
            ),

            value: microphoneEnabled,

            onChanged: (value) {

              setState(() {
                microphoneEnabled = value;
              });

              save(
                "microphone",
                value,
              );

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Notifications",
            ),

            value:
                notificationsEnabled,

            onChanged: (value) {

              setState(() {

                notificationsEnabled =
                    value;

              });

              save(
                "notifications",
                value,
              );

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Alerte batterie faible",
            ),

            value: batteryAlert,

            onChanged: (value) {

              setState(() {

                batteryAlert = value;

              });

              save(
                "battery",
                value,
              );

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Alerter les volontaires",
            ),

            value:
                volunteerAlert,

            onChanged: (value) {

              setState(() {

                volunteerAlert =
                    value;

              });

              save(
                "volunteers",
                value,
              );

            },

          ),

          SwitchListTile(

            title:
                const Text(
              "Mode sombre",
            ),

            value: darkMode,

            onChanged: (value) {

              setState(() {

                darkMode = value;

              });

              save(
                "dark",
                value,
              );

            },

          ),

        ],

      ),

    );

  }

}
import 'package:firebase_auth/firebase_auth.dart';
import 'package:cloud_firestore/cloud_firestore.dart';

class AuthService {

  final FirebaseAuth _auth =
      FirebaseAuth.instance;

  final FirebaseFirestore _firestore =
      FirebaseFirestore.instance;

  /// Utilisateur connecté
  User? get currentUser => _auth.currentUser;

  /// Création d'un compte
  Future<UserCredential> register({

    required String email,
    required String password,
    required String name,

  }) async {

    final credential =
        await _auth.createUserWithEmailAndPassword(

      email: email,
      password: password,

    );

    await _firestore
        .collection("users")
        .doc(credential.user!.uid)
        .set({

      "uid": credential.user!.uid,

      "name": name,

      "email": email,

      "createdAt":
          FieldValue.serverTimestamp(),

      "latitude": null,

      "longitude": null,

      "photoUrl": "",

      "volunteer": false,

      "verified": false,

      "available": true,

      "battery": 100,

    });

    return credential;

  }

  /// Connexion

  Future<UserCredential> login({

    required String email,

    required String password,

  }) {

    return _auth.signInWithEmailAndPassword(

      email: email,

      password: password,

    );

  }

  /// Déconnexion

  Future<void> logout() {

    return _auth.signOut();

  }

  /// Profil utilisateur

  Future<DocumentSnapshot<Map<String, dynamic>>>
      getProfile() {

    return _firestore

        .collection("users")

        .doc(currentUser!.uid)

        .get();

  }

  /// Mise à jour du profil

  Future<void> updateProfile({

    required Map<String, dynamic> data,

  }) async {

    await _firestore

        .collection("users")

        .doc(currentUser!.uid)

        .update(data);

  }

}
import 'package:flutter/material.dart';

import '../../services/auth_service.dart';
import '../home/home_page.dart';

class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {

  final AuthService auth = AuthService();

  final emailController = TextEditingController();

  final passwordController = TextEditingController();

  bool loading = false;

  Future<void> login() async {

    try {

      setState(() {
        loading = true;
      });

      await auth.login(

        email: emailController.text.trim(),

        password: passwordController.text,

      );

      if (!mounted) return;

      Navigator.pushReplacement(

        context,

        MaterialPageRoute(

          builder: (_) => const HomePage(),

        ),

      );

    } catch (e) {

      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(

        SnackBar(

          content: Text(e.toString()),

        ),

      );

    } finally {

      if (mounted) {

        setState(() {

          loading = false;

        });

      }

    }

  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(

        title: const Text(
          "Connexion",
        ),

      ),

      body: Padding(

        padding: const EdgeInsets.all(20),

        child: Column(

          children: [

            const SizedBox(height: 40),

            const Icon(

              Icons.shield,

              size: 100,

              color: Colors.amber,

            ),

            const SizedBox(height: 30),

            TextField(

              controller: emailController,

              keyboardType:
                  TextInputType.emailAddress,

              decoration: const InputDecoration(

                labelText: "Adresse e-mail",

                border: OutlineInputBorder(),

              ),

            ),

            const SizedBox(height: 20),

            TextField(

              controller: passwordController,

              obscureText: true,

              decoration: const InputDecoration(

                labelText: "Mot de passe",

                border: OutlineInputBorder(),

              ),

            ),

            const SizedBox(height: 30),

            SizedBox(

              width: double.infinity,

              height: 55,

              child: ElevatedButton(

                onPressed:
                    loading ? null : login,

                child:

                    loading

                        ? const CircularProgressIndicator()

                        : const Text(

                            "SE CONNECTER",

                          ),

              ),

            ),

            const SizedBox(height: 20),

            TextButton(

              onPressed: () {

                // TODO

                // Ouvrir RegisterPage

              },

              child: const Text(

                "Créer un compte",

              ),

            ),

            TextButton(

              onPressed: () {

                // TODO

                // Mot de passe oublié

              },

              child: const Text(

                "Mot de passe oublié",

              ),

            ),

          ],

        ),

      ),

    );

  }

}
import 'package:flutter/material.dart';

import '../../services/auth_service.dart';
import 'login_page.dart';

class RegisterPage extends StatefulWidget {
  const RegisterPage({super.key});

  @override
  State<RegisterPage> createState() => _RegisterPageState();
}

class _RegisterPageState extends State<RegisterPage> {

  final AuthService auth = AuthService();

  final nameController = TextEditingController();
  final emailController = TextEditingController();
  final passwordController = TextEditingController();
  final confirmPasswordController = TextEditingController();

  bool loading = false;

  Future<void> register() async {

    if (passwordController.text !=
        confirmPasswordController.text) {

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text(
            "Les mots de passe ne correspondent pas.",
          ),
        ),
      );
      return;
    }

    try {

      setState(() {
        loading = true;
      });

      await auth.register(
        name: nameController.text.trim(),
        email: emailController.text.trim(),
        password: passwordController.text,
      );

      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text(
            "Compte créé avec succès.",
          ),
        ),
      );

      Navigator.pushReplacement(
        context,
        MaterialPageRoute(
          builder: (_) => const LoginPage(),
        ),
      );

    } catch (e) {

      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
          content: Text(e.toString()),
        ),
      );

    } finally {

      if (mounted) {
        setState(() {
          loading = false;
        });
      }

    }

  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text("Créer un compte"),
      ),

      body: SingleChildScrollView(

        padding: const EdgeInsets.all(20),

        child: Column(

          children: [

            const SizedBox(height: 20),

            const Icon(
              Icons.person_add,
              size: 90,
              color: Colors.amber,
            ),

            const SizedBox(height: 30),

            TextField(
              controller: nameController,
              decoration: const InputDecoration(
                labelText: "Nom",
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: emailController,
              keyboardType: TextInputType.emailAddress,
              decoration: const InputDecoration(
                labelText: "Adresse e-mail",
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: passwordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: "Mot de passe",
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 15),

            TextField(
              controller: confirmPasswordController,
              obscureText: true,
              decoration: const InputDecoration(
                labelText: "Confirmer le mot de passe",
                border: OutlineInputBorder(),
              ),
            ),

            const SizedBox(height: 30),

            SizedBox(
              width: double.infinity,
              height: 55,
              child: ElevatedButton(
                onPressed: loading ? null : register,
                child: loading
                    ? const CircularProgressIndicator()
                    : const Text(
                        "CRÉER MON COMPTE",
                      ),
              ),
            ),

          ],

        ),

      ),

    );

  }

}
import 'package:flutter/material.dart';

class ProfilePage extends StatefulWidget {
  const ProfilePage({super.key});

  @override
  State<ProfilePage> createState() =>
      _ProfilePageState();
}

class _ProfilePageState
    extends State<ProfilePage> {

  final nameController =
      TextEditingController();

  final phoneController =
      TextEditingController();

  final bloodController =
      TextEditingController();

  final allergyController =
      TextEditingController();

  final treatmentController =
      TextEditingController();

  bool volunteer = false;

  double radius = 5;

  @override
  void initState() {
    super.initState();

    nameController.text = "Mon profil";
    phoneController.text = "";
    bloodController.text = "";
    allergyController.text = "";
    treatmentController.text = "";
  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text("Mon profil"),
      ),

      body: SingleChildScrollView(

        padding:
            const EdgeInsets.all(20),

        child: Column(

          children: [

            const CircleAvatar(
              radius: 60,
              child: Icon(
                Icons.person,
                size: 60,
              ),
            ),

            TextButton(

              onPressed: () {

                // TODO
                // Sélection photo

              },

              child: const Text(
                "Changer la photo",
              ),

            ),

            const SizedBox(height: 20),

            TextField(

              controller: nameController,

              decoration:
                  const InputDecoration(
                labelText: "Nom",
              ),

            ),

            const SizedBox(height: 15),

            TextField(

              controller: phoneController,

              keyboardType:
                  TextInputType.phone,

              decoration:
                  const InputDecoration(
                labelText: "Téléphone",
              ),

            ),

            const SizedBox(height: 15),

            TextField(

              controller: bloodController,

              decoration:
                  const InputDecoration(
                labelText:
                    "Groupe sanguin",
              ),

            ),

            const SizedBox(height: 15),

            TextField(

              controller:
                  allergyController,

              decoration:
                  const InputDecoration(
                labelText:
                    "Allergies",
              ),

            ),

            const SizedBox(height: 15),

            TextField(

              controller:
                  treatmentController,

              decoration:
                  const InputDecoration(
                labelText:
                    "Traitements",
              ),

            ),

            const SizedBox(height: 20),

            SwitchListTile(

              value: volunteer,

              title: const Text(
                "Je suis volontaire",
              ),

              subtitle: const Text(
                "Recevoir les alertes proches",
              ),

              onChanged: (value) {

                setState(() {

                  volunteer = value;

                });

              },

            ),

            const SizedBox(height: 20),

            const Text(
              "Rayon d'intervention",
            ),

            Slider(

              min: 1,

              max: 20,

              divisions: 19,

              value: radius,

              label:
                  "${radius.toInt()} km",

              onChanged: (value) {

                setState(() {

                  radius = value;

                });

              },

            ),

            Text(
              "${radius.toInt()} km",
            ),

            const SizedBox(height: 30),

            SizedBox(

              width: double.infinity,

              child: ElevatedButton(

                onPressed: () {

                  // TODO
                  // Sauvegarder dans Firestore

                },

                child: const Text(
                  "ENREGISTRER",
                ),

              ),

            ),

          ],

        ),

      ),

    );

  }

}
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';

import 'location_service.dart';
import 'notification_service.dart';
import 'sos_service.dart';

class EmergencyAlertService {
  final FirebaseFirestore firestore =
      FirebaseFirestore.instance;

  final FirebaseAuth auth =
      FirebaseAuth.instance;

  final LocationService locationService =
      LocationService();

  final NotificationService notificationService =
      NotificationService();

  final SosService sosService =
      SosService();

  /// Déclenche une alerte complète
  Future<void> triggerEmergency({
    required String emergencyType,
  }) async {

    final user = auth.currentUser;

    if (user == null) {
      throw Exception("Utilisateur non connecté");
    }

    // Position actuelle
    final position =
        await locationService.getCurrentPosition();

    // Création de l'alerte
    await sosService.sendSOS();

    // Notification des contacts
    await notificationService
        .notifyEmergencyContacts(user.uid);

    // Notification des volontaires proches
    await notificationService
        .notifyNearbyVolunteers(
      position.latitude,
      position.longitude,
    );

    // Mise à jour du profil utilisateur
    await firestore
        .collection("users")
        .doc(user.uid)
        .update({

      "lastEmergency":
          FieldValue.serverTimestamp(),

      "lastEmergencyType":
          emergencyType,

      "latitude":
          position.latitude,

      "longitude":
          position.longitude,

    });

    // Notification locale
    await NotificationService
        .showLocalNotification(
      title: "SOS envoyé",
      body:
          "Votre alerte a été transmise.",
    );
  }

  /// Clôture de l'alerte
  Future<void> closeEmergency(
      String alertId) async {

    await firestore
        .collection("alerts")
        .doc(alertId)
        .update({

      "status": "FINISHED",

      "finishedAt":
          FieldValue.serverTimestamp(),

    });
  }
}
import 'package:flutter/material.dart';

class DashboardPage extends StatefulWidget {
  const DashboardPage({super.key});

  @override
  State<DashboardPage> createState() =>
      _DashboardPageState();
}

class _DashboardPageState
    extends State<DashboardPage> {

  int activeAlerts = 3;
  int volunteers = 18;
  int interventions = 12;
  int emergencyContacts = 5;

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      appBar: AppBar(
        title: const Text("Tableau de bord"),
      ),

      body: SingleChildScrollView(

        padding: const EdgeInsets.all(16),

        child: Column(

          children: [

            Row(

              children: [

                Expanded(
                  child: _card(
                    "Alertes actives",
                    "$activeAlerts",
                    Icons.warning,
                    Colors.red,
                  ),
                ),

                const SizedBox(width: 12),

                Expanded(
                  child: _card(
                    "Volontaires",
                    "$volunteers",
                    Icons.people,
                    Colors.green,
                  ),
                ),

              ],

            ),

            const SizedBox(height: 12),

            Row(

              children: [

                Expanded(
                  child: _card(
                    "Mes interventions",
                    "$interventions",
                    Icons.favorite,
                    Colors.orange,
                  ),
                ),

                const SizedBox(width: 12),

                Expanded(
                  child: _card(
                    "Contacts",
                    "$emergencyContacts",
                    Icons.phone,
                    Colors.blue,
                  ),
                ),

              ],

            ),

            const SizedBox(height: 25),

            const Align(

              alignment: Alignment.centerLeft,

              child: Text(

                "Actions rapides",

                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                ),

              ),

            ),

            const SizedBox(height: 15),

            ListTile(
              leading: const Icon(Icons.sos),
              title: const Text("Déclencher un SOS"),
              trailing: const Icon(Icons.arrow_forward_ios),
              onTap: () {},
            ),

            ListTile(
              leading: const Icon(Icons.map),
              title: const Text("Carte"),
              trailing: const Icon(Icons.arrow_forward_ios),
              onTap: () {},
            ),

            ListTile(
              leading: const Icon(Icons.people),
              title: const Text("Communauté"),
              trailing: const Icon(Icons.arrow_forward_ios),
              onTap: () {},
            ),

            ListTile(
              leading: const Icon(Icons.history),
              title: const Text("Historique"),
              trailing: const Icon(Icons.arrow_forward_ios),
              onTap: () {},
            ),

            const SizedBox(height: 25),

            const Align(

              alignment: Alignment.centerLeft,

              child: Text(

                "Dernières notifications",

                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                ),

              ),

            ),

            const SizedBox(height: 15),

            Card(
              child: ListTile(
                leading: const Icon(
                  Icons.notifications,
                  color: Colors.red,
                ),
                title: const Text(
                  "Alerte SOS à 850 m",
                ),
                subtitle: const Text(
                  "Il y a 2 minutes",
                ),
              ),
            ),

            Card(
              child: ListTile(
                leading: const Icon(
                  Icons.check_circle,
                  color: Colors.green,
                ),
                title: const Text(
                  "Intervention terminée",
                ),
                subtitle: const Text(
                  "Aujourd'hui",
                ),
              ),
            ),

          ],

        ),

      ),

    );

  }

  Widget _card(
    String title,
    String value,
    IconData icon,
    Color color,
  ) {

    return Card(

      elevation: 5,

      child: Padding(

        padding: const EdgeInsets.all(18),

        child: Column(

          children: [

            Icon(
              icon,
              size: 40,
              color: color,
            ),

            const SizedBox(height: 10),

            Text(
              value,
              style: const TextStyle(
                fontSize: 28,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 5),

            Text(
              title,
              textAlign: TextAlign.center,
            ),

          ],

        ),

      ),

    );

  }

}
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';

class MapService {
  final FirebaseFirestore _firestore =
      FirebaseFirestore.instance;

  /// Flux des alertes actives
  Stream<QuerySnapshot<Map<String, dynamic>>>
      activeAlerts() {
    return _firestore
        .collection("alerts")
        .where("status", isEqualTo: "ACTIVE")
        .snapshots();
  }

  /// Convertit les alertes Firestore en marqueurs Google Maps
  Future<Set<Marker>> getAlertMarkers() async {

    final snapshot = await _firestore
        .collection("alerts")
        .where("status", isEqualTo: "ACTIVE")
        .get();

    Set<Marker> markers = {};

    for (final doc in snapshot.docs) {

      final data = doc.data();

      final latitude =
          (data["latitude"] as num).toDouble();

      final longitude =
          (data["longitude"] as num).toDouble();

      markers.add(

        Marker(

          markerId: MarkerId(doc.id),

          position: LatLng(
            latitude,
            longitude,
          ),

          icon: BitmapDescriptor.defaultMarkerWithHue(
            BitmapDescriptor.hueRed,
          ),

          infoWindow: InfoWindow(
            title: "🚨 SOS",
            snippet:
                data["type"] ?? "Urgence",
          ),

        ),

      );

    }

    return markers;
  }

  /// Met fin à une alerte
  Future<void> closeAlert(
      String alertId) async {

    await _firestore
        .collection("alerts")
        .doc(alertId)
        .update({

      "status": "FINISHED",

      "finishedAt":
          FieldValue.serverTimestamp(),

    });

  }

}
