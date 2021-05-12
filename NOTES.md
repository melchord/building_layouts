# Notes: Building Layouts

Full credit to Flutter.dev's 'Building layouts' tutorial, these are my personal notes.
https://flutter.dev/docs/development/ui/layout/tutorial#step-5-implement-the-image-section

## Step 0: Create the app base code

Create a basic "Hello World" Flutter app in your chosen IDE and change the app bar title and app title:

```dart
...
Widget build(BuildContext context) {
  return MaterialApp(
    title: 'Flutter layout demo',
    home: Scaffold(
      appBar: AppBar(
        title: Text('Flutter layout demo),
      ).
    ),
    body: ...,
  )
}
...
```

## Step 1: Diagram the layout

From here you would look onto the layout you have been provided and seperate sections into rows and columns.

## Step 2: Implement the title row

First, we build the left column in the title section. Add the following code...

```dart
Widget build(BuildContext context) {
  Widget titleSection = Container(
    padding: const EdgeInsets.all(32),
    children: [
      Expanded(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Container(
              padding: const EdgeInsets.only(bottom: 8),
              child: Text(
                'Oeschinen Lake Campground',
                style: TextStyle(
                  fontWeight: FontWeight.bold,
                ),
              ),
            ),
            Text(
              'Kandersteg, Switzerland',
              style: TextStyle(
                color: Colors.grey[500],
              ),
            ),
          ],
        ),
      ),
      Icon(
        Icons.star,
        color: Colors.red[500],
      ),
    Text('41'),
    ],
  );
};
```

Let's desect what is happening here:

```dart
Expanded(
  child: Column(
    crissAxisAlignment: CrossAxis Alignment.start
    ...
  )
)
```

Putting a **Column** inside an **Expanded** widget stretches the column to use alll remaining free space in the row. Setting **crossAxisAlignment** property to **CrossAxisAlignment.start** positions the column at the start of the row

```dart
  Container(
    padding: const EdgeInsets.only(bottom: 8),
    child: Text(
      'Oeschinen Lake Campground',
      ...
    )
  ),
  Text(
    'Kandersteg, Switzerland',
    style: TextStyle(
      color: Colors.grey[500]
    )
  )
```

Putting the first row of text inside a **Container** enables you to add padding. The second child in the **Column**, also text, will display as grey

```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
Text('41'),
```

The last two items in the title row are a star icon, painted red, and the text '41'. The entire row is in a Container and padded along each edge by 32 pixels.

Lastly, we should add the title section to the app body:

```dart
return MaterialApp(
  ...
  body: Column(
    children: [
      titleSection,
    ],
  ),
  ...
)
```

## Step 3: Implement the button row

The button section contains 3 columns that use the same layout - an icon over a row of text. The columns in this row are evenly spaced, and the text and icons are painted with the primary color.

A helper method will be useful here:

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build (BuildContext context) {
   ...
  }

  Column _buildBottonColumn(Color color, IconData icon, String label) {
    return Column(
      mainAxisSize: MainAxisSize.min,
      mainAxisAlignment: MainAxisAlignment.center,
      children: [
        Icon(icon, color: color),
        Container(
          margin: const EdgeInsets.only(top: 8),
          child: Text(
            label,
            style: TextStyle(
              fontSize: 12,
              fontWeight: FontWeight.w400,
              color: color,
            ),
          ),
        ),
      ],
    );
  }
}
```

The function will add the icon directly to the column. The text is inside a Container with a top-only margin, separating the text from the icon.

With this helper function, build the row containing three columns.

```dart
Widget build(BuildContext context) {
  Widget title Section = Container(...),

  Color color = Theme.of(Context). primaryColor;

  Widget buttonSection = Container(
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        _buildButtonColumn(color, Icons.call, 'CALL'),
        _buildButtonColumn(color, Icons.near_me, 'ROUTE'),
        _buildButtonColumn(color, Icons.share, 'SHARE'),
      ],
    ),
  );
}
```

Add the button section to the body:

```dart
return MaterialApp(
  title: 'Flutter layout demo',
  home: Scaffold(
    body: Column(
      children: [
        titleSection,
        buttonSection,
      ],
    ),
  ),
);
```

## Step 4: Implment the text section

Define the text section as a variable.

```dart
  Widget build(BuildContext context) {
    Widget titlesection = Container(...)

    Widget text Section = Container (
      padding: const EdgeInsets.all(32),
      child: Text(
        'Lake Oeschinen lies at the foot of the Bluemlisalp in the Barnese '
          'Alps. Situated 1,578 meters above sea level, it is one of the '
          'larger Alpine Lakes. A gondola ride from Kandersteg, following by a '
          ' half-hour walk through pastures and pine forest, leads you to the '
          'lake, which warms to 20 degrees Celsius in the summer. Activities '
          'enjoyed here include rowing, and riding the summer toboggan run.',
        softWrap: true,
      ),
    );

    ...
  }
```

By setting softwrap to true, text lines will fill the column width before wrapping at a word boundary.

Add the text section to the body:

```dart
return MaterialApp(
  title: 'Flutter layout demo',
  home: Scaffold(
    children: [
      titleSection,
      buttonSection,
      textSection,
    ]
  )
)
```

## Step 5: Implment the image

Create an **images** directory in the root directory.
Add image, for this we used 'lake.jpg'

Update the **pubspec.yaml** file to include an **assets** tag.
This makes the image available to your code.

```yaml
flutter:
  uses-material-design: true
  assets:
    - images/lake.jpg
```

Now we can reference the image from the code:

```dart
  body: Column(
    children: [
      Image.asset(
        'images/lake.jpg',
        width: 600,
        height: 240,
        fit: BoxFit.cover,
      ),
      titleSection,
      buttonSection,
      textSection,
    ]
  )
```

BoxFit.cover tells the framework that the image should be as small as possible but cover its entire render box.

## Step 6: Final touch

In this final step, we are arranging all of the elements in a ListView, rather than a Column. A ListView supports app body scrolling when the app is run on a small device.

```dart
return MaterialApp(
  title: 'Flutter layout demo',
  home: ...,
  body: ListView(
    ...
  )
)
```

This is the end of the tutorial.
