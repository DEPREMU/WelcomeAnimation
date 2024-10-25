# Welcome Component

A simple React Native component that showcases an animated welcome screen with page navigation. This component utilizes `react-native-reanimated` for smooth animations and gesture handling.

## Features

- Animated transitions between pages.
- Swipe navigation to switch between pages.
- A "Skip" button to jump directly to the last page.
- Dynamic page indicators.

## Installation

Make sure you have `react-native-reanimated` installed in your React Native project:

```bash
npm install react-native-reanimated
```

## Usage

To use the `Welcome` component, simply import it into your main application file or another component:

```javascript
import Welcome from './path/to/Welcome';
```

Then, include it in your render method:

```javascript
const App = () => {
  return (
    <Welcome />
  );
};
```

## Code Explanation

### Main Component

```javascript
const Welcome = () => {
  // State variables
  const maxPages = 4;
  const [page, setPage] = useState(1);
  const [loading, setLoading] = useState(true);

  // Animation values for each page
  const animationsPages = [
    useSharedValue(height * 2),
    useSharedValue(-width),
    useSharedValue(-width),
    useSharedValue(-height * 1.5),
  ];

  const focusAnimation = Array.from({ length: maxPages }, () =>
    useSharedValue(1)
  );

  // Functions to navigate between pages
  const next = () => setPage((prev) => (prev < maxPages ? prev + 1 : prev));
  const less = () => setPage((prev) => (prev > 1 ? prev - 1 : prev));
```

### Gesture Handling

The `PanResponder` is used to detect swipe gestures for navigating between pages.

```javascript
const panResponder = useRef(
  PanResponder.create({
    onMoveShouldSetPanResponder: (evt, gestureState) =>
      Math.abs(gestureState.dx) > 20 || Math.abs(gestureState.dy) > 80,
    onPanResponderRelease: (evt, gestureState) => {
      if (gestureState.dx > 20) less();
      else if (gestureState.dx < -20) next();
      else if (gestureState.dy > 100 || gestureState.dy < -100)
        setPage(maxPages);
    },
  })
).current;
```

### Rendering Pages

Each page is represented as an `Animated.View`, and only the active page is displayed based on the current state.

```javascript
const Page1 = () => (
  <Animated.View {...panResponder.panHandlers} style={[styles.animationView, { transform: [{ translateY: animationsPages[0] }] }]}>
    <SkipButton text="Skip" />
    <Text style={styles.textPoint}>Page 1</Text>
  </Animated.View>
);
```

### Styling

The styles are defined using `StyleSheet` to ensure consistent design.

```javascript
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "violet",
    paddingTop: 40,
    alignItems: "center",
    justifyContent: "center",
  },
  // Additional styles...
});
```
