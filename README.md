import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Login Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const LoginPage(),
    );
  }
}

class LoginPage extends StatefulWidget {
  const LoginPage({super.key});

  @override
  State<LoginPage> createState() => _LoginPageState();
}

class _LoginPageState extends State<LoginPage> {
  // مفتاح النموذج (Form Key) لإدارة حالة النموذج والتحقق من صحة المدخلات
  final _formKey = GlobalKey<FormState>();

  // وحدات التحكم (Controllers) للحصول على قيمة حقول النصوص
  final TextEditingController _emailController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();

  // دالة لمعالجة عملية تسجيل الدخول
  void _login() {
    // التحقق من صحة المدخلات
    if (_formKey.currentState!.validate()) {
      // إذا كانت المدخلات صحيحة، يمكنك تنفيذ منطق تسجيل الدخول هنا
      final String email = _emailController.text;
      final String password = _passwordController.text;

      // طباعة القيم للمعاينة (في التطبيق الحقيقي سيتم إرسالها إلى خادم)
      print('Email: $email');
      print('Password: $password');

      // عرض رسالة نجاح بسيطة
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('تم محاولة تسجيل الدخول بنجاح!')),
      );
      
      // هنا يمكنك التوجيه إلى الشاشة الرئيسية (Home Screen)
      // مثال: Navigator.pushReplacement(context, MaterialPageRoute(builder: (context) => const HomeScreen()));
    }
  }

  @override
  void dispose() {
    // التخلص من وحدات التحكم عند إغلاق الودجت
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('تسجيل الدخول'),
        centerTitle: true,
      ),
      // استخدام SingleChildScrollView لمنع خطأ الـ Overflow عند ظهور لوحة المفاتيح
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(24.0),
        child: Form(
          key: _formKey, // ربط مفتاح النموذج
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: <Widget>[
              // [Image of Flutter login screen with email and password fields]
              
              // 1. حقل إدخال البريد الإلكتروني (Email)
              TextFormField(
                controller: _emailController,
                keyboardType: TextInputType.emailAddress,
                decoration: const InputDecoration(
                  labelText: 'البريد الإلكتروني',
                  hintText: 'أدخل بريدك الإلكتروني',
                  prefixIcon: Icon(Icons.email),
                  border: OutlineInputBorder(),
                ),
                // التحقق من صحة المدخلات (Validator)
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'الرجاء إدخال البريد الإلكتروني';
                  }
                  // يمكنك إضافة المزيد من التحقق هنا
                  return null;
                },
              ),
              
              const SizedBox(height: 16.0), // مسافة بين الحقول
              
              // 2. حقل إدخال كلمة المرور (Password)
              TextFormField(
                controller: _passwordController,
                obscureText: true, // لإخفاء النص
                decoration: const InputDecoration(
                  labelText: 'كلمة المرور',
                  hintText: 'أدخل كلمة المرور',
                  prefixIcon: Icon(Icons.lock),
                  border: OutlineInputBorder(),
                ),
                // التحقق من صحة المدخلات (Validator)
                validator: (value) {
                  if (value == null || value.isEmpty) {
                    return 'الرجاء إدخال كلمة المرور';
                  }
                  if (value.length < 6) {
                    return 'يجب أن تكون كلمة المرور 6 أحرف على الأقل';
                  }
                  return null;
                },
              ),

              const SizedBox(height: 30.0), // مسافة قبل الزر

              // 3. زر تسجيل الدخول (Login Button)
              ElevatedButton(
                onPressed: _login, // استدعاء دالة تسجيل الدخول
                style: ElevatedButton.styleFrom(
                  padding: const EdgeInsets.symmetric(vertical: 15.0),
                  textStyle: const TextStyle(fontSize: 18),
                ),
                child: const Text('تسجيل الدخول'),
              ),
              
              const SizedBox(height: 10.0),

              // 4. زر أو نص "نسيت كلمة المرور" (Forgot Password)
              TextButton(
                onPressed: () {
                  // منطق التوجيه إلى شاشة استعادة كلمة المرور
                  print('Forgot Password pressed');
                },
                child: const Text('نسيت كلمة المرور؟'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
