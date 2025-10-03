import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => CounterModel()),
        ChangeNotifierProvider(create: (_) => UserModel()),
        ChangeNotifierProvider(create: (_) => ThemeModel()),
        ChangeNotifierProvider(create: (_) => TodoListModel()),
      ],
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    final themeModel = context.watch<ThemeModel>();
    return MaterialApp(
      title: 'Lab 6 - State Management Demo',
      theme: themeModel.isDark ? ThemeData.dark() : ThemeData.light(),
      home: const HomeScreen(),
    );
  }
}

/// =======================
/// Models for Provider section
/// =======================
class CounterModel extends ChangeNotifier {
  int _count = 0;
  int get count => _count;
  void increment() {
    _count++;
    notifyListeners();
  }

  void decrement() {
    _count--;
    notifyListeners();
  }

  void reset() {
    _count = 0;
    notifyListeners();
  }
}

class UserModel extends ChangeNotifier {
  String _username = 'Guest';
  String get username => _username;
  void setAdmin() {
    _username = 'Admin';
    notifyListeners();
  }

  void changeName(String newName) {
    _username = newName;
    notifyListeners();
  }
}

class ThemeModel extends ChangeNotifier {
  bool _isDark = false;
  bool get isDark => _isDark;

  void toggle() {
    _isDark = !_isDark;
    notifyListeners();
  }
}

class TodoListModel extends ChangeNotifier {
  final List<String> _tasks = [];
  List<String> get tasks => List.unmodifiable(_tasks);
  void addTask(String task) {
    if (task.trim().isEmpty) return;
    _tasks.add(task);
    notifyListeners();
  }

  void removeTask(int index) {
    if (index < 0 || index >= _tasks.length) return;
    _tasks.removeAt(index);
    notifyListeners();
  }
}

/// =======================
/// Home / Navigation
/// =======================
class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    final user = context.watch<UserModel>();
    return Scaffold(
      appBar: AppBar(title: const Text('Lab 6 — State Examples')),
      body: ListView(
        padding: const EdgeInsets.all(12),
        children: [
          Text('Logged in as: ${user.username}',
              style: Theme.of(context).textTheme.titleMedium),
          const SizedBox(height: 12),
          _menuButton(context, '1. Stateless vs Stateful', StatelessVsStatefulScreen()),
          _menuButton(context, '2. setState Counter (+/-)', CounterSetStateScreen()),
          _menuButton(context, '3. Visibility Toggle', VisibilityToggleScreen()),
          _menuButton(context, '4. Add items to List', AddItemListScreen()),
          _menuButton(context, '5. Color Box', ColorBoxScreen()),
          _menuButton(context, '6. Like Button (interactive)', LikeButtonScreen()),
          _menuButton(context, '7. TextFormField & live username', LiveTextFormFieldScreen()),
          _menuButton(context, '8. Lifting State Up (Switch Manager)', SwitchManagerScreen()),
          _menuButton(context, '9. Provider Counter', ProviderCounterScreen()),
          _menuButton(context, '10. Provider User & Theme', ProviderUserThemeScreen()),
          _menuButton(context, '11. Todo with Provider', ProviderTodoScreen()),
        ],
      ),
    );
  }

  Widget _menuButton(BuildContext context, String title, Widget screen) {
    return Card(
      child: ListTile(
        title: Text(title),
        trailing: const Icon(Icons.arrow_forward_ios, size: 16),
        onTap: () => Navigator.of(context).push(
          MaterialPageRoute(builder: (_) => screen),
        ),
      ),
    );
  }
}

/// =======================
/// 1. Stateless vs Stateful
/// =======================
class StatelessVsStatefulScreen extends StatelessWidget {
  const StatelessVsStatefulScreen({super.key});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Stateless vs Stateful')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(
          children: const [
            StaticProfileCard(),
            SizedBox(height: 12),
            InteractiveProfileCard(),
            SizedBox(height: 12),
            ProductCard(productName: 'Widget Pro', price: 19.99),
          ],
        ),
      ),
    );
  }
}

class StaticProfileCard extends StatelessWidget {
  const StaticProfileCard({super.key});
  @override
  Widget build(BuildContext context) {
    return const Card(
      child: ListTile(
        leading: Icon(Icons.person),
        title: Text('John Doe'),
        subtitle: Text('john.doe@email.com'),
      ),
    );
  }
}

class InteractiveProfileCard extends StatefulWidget {
  const InteractiveProfileCard({super.key});
  @override
  State<InteractiveProfileCard> createState() => _InteractiveProfileCardState();
}

class _InteractiveProfileCardState extends State<InteractiveProfileCard> {
  // Example state field (not interactive here yet)
  int taps = 0;

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: const Icon(Icons.person_outline),
        title: const Text('Jane Smith'),
        subtitle: Text('Taps: $taps'),
        onTap: () => setState(() => taps++),
      ),
    );
  }
}

class ProductCard extends StatelessWidget {
  final String productName;
  final double price;
  const ProductCard({super.key, required this.productName, required this.price});

  @override
  Widget build(BuildContext context) {
    return Card(
      child: ListTile(
        leading: const Icon(Icons.shopping_bag),
        title: Text(productName),
        subtitle: Text('\$${price.toStringAsFixed(2)}'),
      ),
    );
  }
}

/// =======================
/// 2. setState Counter with two FABs
/// =======================
class CounterSetStateScreen extends StatefulWidget {
  const CounterSetStateScreen({super.key});
  @override
  State<CounterSetStateScreen> createState() => _CounterSetStateScreenState();
}

class _CounterSetStateScreenState extends State<CounterSetStateScreen> {
  int _counter = 0;

  void _increment() => setState(() => _counter++);
  void _decrement() => setState(() => _counter--);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Counter using setState')),
      body: Center(
        child: Text('Count: $_counter', style: Theme.of(context).textTheme.headlineMedium),
      ),
      floatingActionButton: Row(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
            heroTag: 'dec',
            onPressed: _decrement,
            child: const Icon(Icons.remove),
          ),
          const SizedBox(width: 12),
          FloatingActionButton(
            heroTag: 'inc',
            onPressed: _increment,
            child: const Icon(Icons.add),
          ),
        ],
      ),
    );
  }
}

/// =======================
/// 3. Visibility toggle
/// =======================
class VisibilityToggleScreen extends StatefulWidget {
  const VisibilityToggleScreen({super.key});
  @override
  State<VisibilityToggleScreen> createState() => _VisibilityToggleScreenState();
}

class _VisibilityToggleScreenState extends State<VisibilityToggleScreen> {
  bool _isVisible = true;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Visibility Toggle')),
      body: Center(
        child: Column(mainAxisAlignment: MainAxisAlignment.center, children: [
          if (_isVisible) const Text('Now you see me!'),
          const SizedBox(height: 12),
          ElevatedButton(
            onPressed: () => setState(() => _isVisible = !_isVisible),
            child: Text(_isVisible ? 'Hide' : 'Show'),
          ),
        ]),
      ),
    );
  }
}

/// =======================
/// 4. List add - TextField + button + ListView
/// =======================
class AddItemListScreen extends StatefulWidget {
  const AddItemListScreen({super.key});
  @override
  State<AddItemListScreen> createState() => _AddItemListScreenState();
}

class _AddItemListScreenState extends State<AddItemListScreen> {
  final List<String> _items = [];
  final TextEditingController _controller = TextEditingController();

  void _addItem() {
    final text = _controller.text.trim();
    if (text.isEmpty) return;
    setState(() {
      _items.add(text);
      _controller.clear();
    });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Add items to list')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(children: [
          Row(children: [
            Expanded(child: TextField(controller: _controller, decoration: const InputDecoration(hintText: 'New item'))),
            const SizedBox(width: 8),
            ElevatedButton(onPressed: _addItem, child: const Text('Add')),
          ]),
          const SizedBox(height: 12),
          Expanded(
            child: ListView.builder(
              itemCount: _items.length,
              itemBuilder: (_, i) => ListTile(
                title: Text(_items[i]),
                trailing: IconButton(
                  icon: const Icon(Icons.delete),
                  onPressed: () => setState(() => _items.removeAt(i)),
                ),
              ),
            ),
          )
        ]),
      ),
    );
  }
}

/// =======================
/// 5. Color Box
/// =======================
class ColorBoxScreen extends StatefulWidget {
  const ColorBoxScreen({super.key});
  @override
  State<ColorBoxScreen> createState() => _ColorBoxScreenState();
}

class _ColorBoxScreenState extends State<ColorBoxScreen> {
  Color _color = Colors.red;

  void _setColor(Color c) {
    setState(() => _color = c);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Color Box')),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Container(width: 200, height: 200, color: _color),
          const SizedBox(height: 12),
          Wrap(spacing: 8, children: [
            ElevatedButton(onPressed: () => _setColor(Colors.red), child: const Text('Red')),
            ElevatedButton(onPressed: () => _setColor(Colors.green), child: const Text('Green')),
            ElevatedButton(onPressed: () => _setColor(Colors.blue), child: const Text('Blue')),
          ])
        ],
      ),
    );
  }
}

/// =======================
/// 6. Like Button (interactive)
/// =======================
class LikeButtonScreen extends StatefulWidget {
  const LikeButtonScreen({super.key});
  @override
  State<LikeButtonScreen> createState() => _LikeButtonScreenState();
}

class _LikeButtonScreenState extends State<LikeButtonScreen> {
  bool _isLiked = false;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Like Button')),
      body: Center(
        child: IconButton(
          iconSize: 64,
          icon: Icon(_isLiked ? Icons.favorite : Icons.favorite_border),
          color: _isLiked ? Colors.red : null,
          onPressed: () => setState(() => _isLiked = !_isLiked),
        ),
      ),
    );
  }
}

/// =======================
/// 7. TextFormField & live update
/// =======================
class LiveTextFormFieldScreen extends StatefulWidget {
  const LiveTextFormFieldScreen({super.key});
  @override
  State<LiveTextFormFieldScreen> createState() => _LiveTextFormFieldScreenState();
}

class _LiveTextFormFieldScreenState extends State<LiveTextFormFieldScreen> {
  String _userName = '';
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Live TextFormField')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(children: [
          TextFormField(
            decoration: const InputDecoration(labelText: 'Enter your name'),
            onChanged: (v) => setState(() => _userName = v),
          ),
          const SizedBox(height: 12),
          Text('Hello, $_userName', style: Theme.of(context).textTheme.titleMedium),
        ]),
      ),
    );
  }
}

/// =======================
/// 8. Lifting state up: SwitchManager
/// =======================
class SwitchManagerScreen extends StatefulWidget {
  const SwitchManagerScreen({super.key});
  @override
  State<SwitchManagerScreen> createState() => _SwitchManagerScreenState();
}

class _SwitchManagerScreenState extends State<SwitchManagerScreen> {
  bool _isActive = false;

  void _handleSwitchChanged(bool val) => setState(() => _isActive = val);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Lifting State Up — Switch Manager')),
      body: Center(
        child: Column(mainAxisAlignment: MainAxisAlignment.center, children: [
          Text('The Switch is: ${_isActive ? "ON" : "OFF"}'),
          const SizedBox(height: 12),
          InteractiveSwitch(isActive: _isActive, onChanged: _handleSwitchChanged),
        ]),
      ),
    );
  }
}

class InteractiveSwitch extends StatelessWidget {
  final bool isActive;
  final ValueChanged<bool> onChanged;
  const InteractiveSwitch({super.key, required this.isActive, required this.onChanged});

  @override
  Widget build(BuildContext context) {
    return Switch(value: isActive, onChanged: onChanged);
  }
}

/// =======================
/// 9. Provider counter
/// =======================
class ProviderCounterScreen extends StatelessWidget {
  const ProviderCounterScreen({super.key});
  @override
  Widget build(BuildContext context) {
    final counter = context.watch<CounterModel>();
    return Scaffold(
      appBar: AppBar(title: const Text('Provider Counter')),
      body: Center(child: Text('Count: ${counter.count}', style: Theme.of(context).textTheme.headlineMedium)),
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
            heroTag: 'p_inc',
            onPressed: () => context.read<CounterModel>().increment(),
            child: const Icon(Icons.add),
          ),
          const SizedBox(height: 8),
          FloatingActionButton(
            heroTag: 'p_dec',
            onPressed: () => context.read<CounterModel>().decrement(),
            child: const Icon(Icons.remove),
          ),
          const SizedBox(height: 8),
          FloatingActionButton(
            heroTag: 'p_reset',
            onPressed: () => context.read<CounterModel>().reset(),
            child: const Icon(Icons.refresh),
          ),
        ],
      ),
    );
  }
}

/// =======================
/// 10. Provider user & theme
/// =======================
class ProviderUserThemeScreen extends StatelessWidget {
  const ProviderUserThemeScreen({super.key});
  @override
  Widget build(BuildContext context) {
    final user = context.watch<UserModel>();
    final themeModel = context.watch<ThemeModel>();
    return Scaffold(
      appBar: AppBar(title: const Text('Provider: User & Theme')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(children: [
          Text('Username: ${user.username}', style: Theme.of(context).textTheme.titleMedium),
          const SizedBox(height: 8),
          ElevatedButton(
            onPressed: () => context.read<UserModel>().setAdmin(),
            child: const Text('Make Admin'),
          ),
          const SizedBox(height: 16),
          Row(mainAxisAlignment: MainAxisAlignment.spaceBetween, children: [
            const Text('Dark mode'),
            Switch(value: themeModel.isDark, onChanged: (_) => context.read<ThemeModel>().toggle()),
          ])
        ]),
      ),
    );
  }
}

/// =======================
/// 11. Todo with Provider
/// =======================
class ProviderTodoScreen extends StatefulWidget {
  const ProviderTodoScreen({super.key});
  @override
  State<ProviderTodoScreen> createState() => _ProviderTodoScreenState();
}

class _ProviderTodoScreenState extends State<ProviderTodoScreen> {
  final TextEditingController _taskController = TextEditingController();

  @override
  void dispose() {
    _taskController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final todo = context.watch<TodoListModel>();
    return Scaffold(
      appBar: AppBar(title: const Text('Todo List (Provider)')),
      body: Padding(
        padding: const EdgeInsets.all(12),
        child: Column(children: [
          Row(children: [
            Expanded(child: TextField(controller: _taskController, decoration: const InputDecoration(hintText: 'Enter task'))),
            const SizedBox(width: 8),
            ElevatedButton(
              onPressed: () {
                context.read<TodoListModel>().addTask(_taskController.text);
                _taskController.clear();
              },
              child: const Text('Add'),
            )
          ]),
          const SizedBox(height: 12),
          Expanded(
            child: ListView.builder(
              itemCount: todo.tasks.length,
              itemBuilder: (_, i) => ListTile(
                title: Text(todo.tasks[i]),
                trailing: IconButton(
                  icon: const Icon(Icons.delete),
                  onPressed: () => context.read<TodoListModel>().removeTask(i),
                ),
              ),
            ),
          )
        ]),
      ),
    );
  }
}
