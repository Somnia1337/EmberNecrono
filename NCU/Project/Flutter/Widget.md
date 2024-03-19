### NavigationBar

```dart
NavigationBar(
  destinations: [
    NavigationDestination(
      icon: Icon(Icons.home),
      label: 'Home',
      ),
    NavigationDestination(
      icon: Icon(Icons.settings_rounded),
      label: 'Settings',
      ),
    ]
    selectedIndex: currentPageIndex
    onDestinationSelected: (int index) {
	  setState(() {
	    currentPageIndex = index;
	  });
    }
    animationDuration: Duration(ms: 1000)
)
```

```dart
Scaffold(
  bottomNavigationBar: NavigationBar(...),
  body: [Widget1, Widget2, ...][currentPageIndex]
)
```