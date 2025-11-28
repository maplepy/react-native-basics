
---
title: "React Native UI snippets (NativeWind / Tailwind)"
description: "Common React Native primitives with NativeWind (Tailwind) examples, tips, and gotchas. Obsidian-friendly headings and a short TOC."
---

# React Native primitives — NativeWind / Tailwind examples

This page collects concise examples, notes, and best practices for common React Native primitives using NativeWind (Tailwind-style classes). It's organised for easy browsing in Obsidian or other markdown readers.

Table of contents
- [React Native primitives — NativeWind / Tailwind examples](#react-native-primitives--nativewind--tailwind-examples)
  - [Layout Components](#layout-components)
    - [View](#view)
    - [ScrollView](#scrollview)
    - [SafeAreaView](#safeareaview)
  - [Interactive Components](#interactive-components)
    - [TouchableOpacity](#touchableopacity)
    - [TouchableHighlight](#touchablehighlight)
    - [TouchableWithoutFeedback](#touchablewithoutfeedback)
    - [Switch](#switch)
  - [List Components](#list-components)
    - [FlatList](#flatlist)
    - [FlatList vs map](#flatlist-vs-map)
  - [Media Components](#media-components)
    - [Image](#image)
    - [ImageBackground](#imagebackground)
  - [Feedback Components](#feedback-components)
    - [ActivityIndicator](#activityindicator)
    - [Alert](#alert)
    - [Modal](#modal)
  - [System Components](#system-components)
    - [StatusBar](#statusbar)
  - [Quick tips \& gotchas](#quick-tips--gotchas)

---

## Layout Components

### View

Use `View` to build layout structures. React Native uses flexbox by default.

- Common props: `style`, `pointerEvents`, `accessibility*` props.
- Use NativeWind's `styled` helper to apply Tailwind classes.

Example (NativeWind):

```tsx
import { View, Text } from 'react-native';
import { styled } from 'nativewind';

const StyledView = styled(View);

export default function Example() {
  return (
    <StyledView className="flex-1 justify-center items-center bg-gray-100">
      <StyledView className="p-4 bg-white rounded shadow">
        <Text className="text-lg font-bold">Hello, World!</Text>
      </StyledView>
    </StyledView>
  );
}
```

Notes:
- Prefer `View` + flex classes for layout over fixed positions when possible.
- Keep deeply nested views shallow to avoid layout complexity.

### ScrollView

Use `ScrollView` for scrollable content that isn't long enough to need virtualization (or when children are small in number).

```tsx
import { ScrollView, Text, View } from 'react-native';
import { styled } from 'nativewind';

const StyledScrollView = styled(ScrollView);
const StyledView = styled(View);

<StyledScrollView className="flex-1 bg-gray-100">
  {Array.from({ length: 20 }).map((_, i) => (
    <StyledView key={i} className="p-4 border-b">
      <Text>Item {i + 1}</Text>
    </StyledView>
  ))}
</StyledScrollView>
```

Tip: Avoid putting very long lists inside a `ScrollView`—use `FlatList` for large datasets.

### SafeAreaView

Wrap content to avoid notches, rounded corners, and system UI.

```tsx
import { SafeAreaView, Text } from 'react-native';
import { styled } from 'nativewind';

const StyledSafeAreaView = styled(SafeAreaView);

<StyledSafeAreaView className="flex-1 bg-white">
  <Text className="text-center text-lg">This is within the safe area!</Text>
</StyledSafeAreaView>
```

Sometimes it does fall short for some devices, in that case you can use the `react-native-safe-area-context` library for more robust handling of safe areas.

---

## Interactive Components

React Native provides several primitives for touch interaction. Use `onPress` (not `onClick`).

### TouchableOpacity

Gives an opacity feedback when pressed. Good for simple buttons.

```tsx
import { TouchableOpacity, Text } from 'react-native';
import { styled } from 'nativewind';

const StyledTouchableOpacity = styled(TouchableOpacity);

<StyledTouchableOpacity
  className="bg-blue-500 p-4 rounded"
  onPress={() => alert('Pressed!')}
>
  <Text className="text-white text-center">Press Me</Text>
</StyledTouchableOpacity>
```

### TouchableHighlight

Shows a highlight (underlay) color while pressed. Useful when you want a colored feedback overlay.

```tsx
import { TouchableHighlight, Text } from 'react-native';
import { styled } from 'nativewind';

const StyledTouchableHighlight = styled(TouchableHighlight);

<StyledTouchableHighlight
  className="bg-green-500 p-4 rounded"
  onPress={() => alert('Pressed!')}
  underlayColor="#34D399"
>
  <Text className="text-white text-center">Press Me</Text>
</StyledTouchableHighlight>
```

### TouchableWithoutFeedback

No visual feedback; useful for gestures where you don't want UI change (e.g., dismissing a keyboard).

```tsx
import { TouchableWithoutFeedback, Keyboard } from 'react-native';
import { View, TextInput } from 'react-native';
import { styled } from 'nativewind';

const StyledTouchableWithoutFeedback = styled(TouchableWithoutFeedback);
const StyledView = styled(View);
const StyledTextInput = styled(TextInput);

<StyledTouchableWithoutFeedback onPress={Keyboard.dismiss}>
  <StyledView className="flex-1 justify-center items-center bg-gray-100">
    <StyledTextInput className="border p-2 w-64" placeholder="Tap outside to dismiss keyboard" />
  </StyledView>
</StyledTouchableWithoutFeedback>
```

Tip: For complex gestures, consider `Pressable` (more control) or gesture libraries for advanced interactions.

### Switch

Use `Switch` for toggleable on/off states.

```tsx
import { Switch, View } from 'react-native';
import { styled } from 'nativewind';
const StyledSwitch = styled(Switch);
const StyledView = styled(View);
<StyledView className="flex-1 justify-center items-center">
  <StyledSwitch
    value={isEnabled}
    onValueChange={toggleSwitch}
    className="transform scale-125"
  />
</StyledView>
```

---

## List Components

For long lists prefer `FlatList` (virtualised) rather than rendering arrays with `.map()`.

### FlatList

```tsx
import React from 'react';
import { FlatList, Text, View } from 'react-native';

const DATA = [
  { id: '1', title: 'Item 1' },
  { id: '2', title: 'Item 2' },
  { id: '3', title: 'Item 3' },
];

const renderItem = ({ item }: { item: { id: string; title: string } }) => (
  <View style={{ padding: 16, borderBottomWidth: 1, borderBottomColor: '#e5e7eb' }}>
    <Text>{item.title}</Text>
  </View>
);

export default function ListExample() {
  return (
    <FlatList
      data={DATA}
      renderItem={renderItem}
      keyExtractor={(item) => item.id}
      ItemSeparatorComponent={() => <View style={{ height: 1, backgroundColor: '#f3f4f6' }} />}
    />
  );
}
```

When using NativeWind, you can replace inline styles with `styled` wrappers and `className`.

### FlatList vs map

Considerations:

- FlatList: virtualised, low memory, ideal for 100+ items, supports pagination (`onEndReached`), `refreshControl`, headers/footers.
- Array.map: simple, good for small fixed lists (<100). Renders everything at once (may cause jank on large lists).

Use FlatList when scroll performance or memory is a concern.

---

## Media Components

### Image

Use `Image` to display images from local or remote sources.

```tsx
import { Image, View } from 'react-native';
import { styled } from 'nativewind';
const StyledImage = styled(Image);
const StyledView = styled(View);
<StyledView className="flex-1 justify-center items-center">
  <StyledImage
    className="w-32 h-32 rounded-full"
    source={{ uri: 'https://example.com/profile.jpg' }}
    resizeMode="cover"
  />
</StyledView>
```

### ImageBackground

Use `ImageBackground` to display an image as a background for other components.

```tsx
import { ImageBackground, Text, View } from 'react-native';
import { styled } from 'nativewind';
const StyledImageBackground = styled(ImageBackground);
const StyledView = styled(View);
<StyledImageBackground
  className="flex-1 justify-center items-center"
  source={{ uri: 'https://example.com/background.jpg' }}
  resizeMode="cover"
>
  <Text className="text-white text-2xl font-bold">Hello, World!</Text>
</StyledImageBackground>
```

They don't support SVG natively; use `react-native-svg` for SVG images.

---

## Feedback Components

### ActivityIndicator

Small built-in spinner for loading states.

```tsx
import { ActivityIndicator, View } from 'react-native';
import { styled } from 'nativewind';

const StyledView = styled(View);

<StyledView className="flex-1 justify-center items-center">
  <ActivityIndicator size="large" color="#3B82F6" />
</StyledView>
```

Use cases:
- Short asynchronous operations.
- For longer loading, pair with skeleton UI or progress indicators.

### Alert

Use `Alert` for simple alert dialogs.

```tsx
import { Alert, Button, View } from 'react-native';

const App = () => {
  const showAlert = () => {
    Alert.alert(
      "Alert Title",
      "My Alert Message",
      [
        { text: "OK", onPress: () => console.log("OK Pressed") }
      ]
    );
  };

  return (
    <View className="flex-1 justify-center items-center">
      <Button title="Show Alert" onPress={showAlert} />
    </View>
  );
};
export default App;
```

### Modal

Use `Modal` to create overlay dialogs or pop-ups.

```tsx
import { Modal, Text, View, Button } from 'react-native';
import { styled } from 'nativewind';
const StyledModal = styled(Modal);
const StyledView = styled(View);
const StyledText = styled(Text);
const StyledButton = styled(Button)
<StyledModal
  animationType="slide"
  transparent={true}
  visible={modalVisible}
  onRequestClose={() => {
    setModalVisible(!modalVisible);
  }}
>
  <StyledView className="flex-1 justify-center items-center bg-black bg-opacity-50">
    <StyledView className="bg-white p-6 rounded shadow-lg">
      <StyledText className="text-lg mb-4">This is a modal!</StyledText>
      <StyledButton title="Close" onPress={() => setModalVisible(!modalVisible)} />
    </StyledView>
  </StyledView>
</StyledModal>
```

---

## System Components

### StatusBar

Both React Native and Expo have their own `StatusBar` component to control the appearance of the device's status bar.

(prefer the Expo one)

```tsx
import { StatusBar } from 'expo-status-bar';
import { styled } from 'nativewind';
const StyledStatusBar = styled(StatusBar);
<StyledStatusBar style="auto" />
```

---

## Quick tips & gotchas

- Use `Pressable` for advanced press states and to reduce scattered touch primitives.
- Keep `keyExtractor` stable for lists to avoid re-renders.
- Prefer `useCallback` for `renderItem` when list re-renders often (but measure—optimisation tradeoffs vary).
- NativeWind supports many Tailwind utilities, but platform differences (iOS/Android) still apply—test on devices.

---
