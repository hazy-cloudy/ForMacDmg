import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:url_launcher/url_launcher.dart';

const String targetUrl = 'https://www.mornhub.help/';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'My Web Launcher',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const LauncherPage(),
    );
  }
}

class LauncherPage extends StatefulWidget {
  const LauncherPage({Key? key}) : super(key: key);

  @override
  State<LauncherPage> createState() => _LauncherPageState();
}

class _LauncherPageState extends State<LauncherPage> {
  bool _launched = false;
  String? _error;

  @override
  void initState() {
    super.initState();
    // Try to open the URL once the app starts. If it fails, show a simple UI
    // that allows the user to retry or copy the URL.
    WidgetsBinding.instance.addPostFrameCallback((_) {
      _openUrl();
    });
  }

  Future<void> _openUrl() async {
    setState(() {
      _error = null;
    });
    final uri = Uri.parse(targetUrl);
    try {
      final success = await launchUrl(uri, mode: LaunchMode.externalApplication);
      setState(() {
        _launched = success;
        if (!success) _error = '无法打开外部浏览器';
      });
    } catch (e) {
      setState(() {
        _error = e.toString();
        _launched = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('My Web Launcher')),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16),
          child: Column(
            mainAxisSize: MainAxisSize.min,
            children: [
              const Icon(Icons.open_in_new, size: 64, color: Colors.blue),
              const SizedBox(height: 12),
              Text(targetUrl, style: Theme.of(context).textTheme.titleMedium),
              const SizedBox(height: 12),
              if (_launched)
                const Text('已在默认浏览器中打开', style: TextStyle(color: Colors.green)),
              if (_error != null) ...[
                Text('打开失败: $_error', style: const TextStyle(color: Colors.red)),
                const SizedBox(height: 8),
              ],
              Row(
                mainAxisSize: MainAxisSize.min,
                children: [
                  ElevatedButton(
                    onPressed: _openUrl,
                    child: const Text('在浏览器中打开'),
                  ),
                  const SizedBox(width: 12),
                  ElevatedButton(
                    onPressed: () {
                      Clipboard.setData(const ClipboardData(text: targetUrl));
                      ScaffoldMessenger.of(context).showSnackBar(const SnackBar(content: Text('已复制链接')));
                    },
                    child: const Text('复制链接'),
                  ),
                ],
              )
            ],
          ),
        ),
      ),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text('You have pushed the button this many times:'),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headlineMedium,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
