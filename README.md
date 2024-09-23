``` import 'package:flutter/material.dart';

class SliderContainer extends StatefulWidget {
  const SliderContainer({
    super.key,
    required this.onChanged,
    this.min = 0.0,
    this.max = 100.0,
    this.initialValue = 0.0,
    this.step = 10.0, // Step size for indicator
  });

  final double min;
  final double max;
  final double initialValue;
  final double step; // Step size for indicator
  final ValueChanged<double> onChanged;
  @override
  SliderContainerState createState() => SliderContainerState();
}

class SliderContainerState extends State<SliderContainer> {
  late double _currentValue;
  @override
  void initState() {
    super.initState();
    _currentValue = widget.initialValue;
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.center,
      children: [
        // Slider
        Container(
          padding: EdgeInsets.all(8),
          margin: const EdgeInsets.only(right: 10),
          child: LayoutBuilder(
            builder: (context, constraints) {
              final screenWidth = constraints.maxWidth;
              // Normalize the current value to calculate its position on the slider
              final normalizedValue = (_currentValue - widget.min) /
                  (widget.max - widget.min); // 50-0/100-0  =
              final filledWidth = normalizedValue * screenWidth;
              const double handleWidth =
                  2.0; // Width of the handle (arrow + vertical line)
              return GestureDetector(
                onHorizontalDragUpdate: (details) {
                  setState(() {
                    // Calculate the new value based on the drag position
                    final newDragPosition =
                        (details.localPosition.dx / screenWidth) *
                                (widget.max - widget.min) +
                            widget.min;
                    // Clamp the value within the bounds of min and max
                    _currentValue =
                        newDragPosition.clamp(widget.min, widget.max);
                    // Call the onChanged callback with the new value
                    widget.onChanged(_currentValue);
                  });
                },
                child: SizedBox(
                  height: 60,
                  child: Stack(
                    children: [
                      Positioned(
                        top: 20,
                        bottom: 0,
                        left: 0,
                        right: 0,
                        child: Container(
                          height: 60,
                          decoration: BoxDecoration(
                            border: Border.all(color: Colors.grey),
                            borderRadius: BorderRadius.circular(8),
                          ),
                          child: Stack(
                            children: [
                              // Blue part that represents the filled value
                              Positioned(
                                left: 0,
                                top: 0,
                                bottom: 0,
                                // Adjust the width of the blue fill to account for half the handle's width
                                width: (filledWidth - (handleWidth / 2)),
                                child: Container(
                                  decoration: const BoxDecoration(
                                    color: Colors.blue,
                                    borderRadius: BorderRadius.only(
                                        topLeft: Radius.circular(8),
                                        bottomLeft: Radius.circular(8)),
                                  ),
                                ),
                              ),
                              // Drag handle with custom icon
                              Positioned(
                                // Ensure the handle is centered over the current value
                                left: filledWidth - (handleWidth / 2) - 3,
                                child: Container(
                                  width: handleWidth,
                                  height: 40,
                                  color:
                                      Colors.black, // Vertical line (divider)
                                ),
                              ),
                            ],
                          ),
                        ),
                      ),
                      Positioned(
                        // Ensure the handle is centered over the current value
                        left: filledWidth - (handleWidth / 2) - 21,
                        top: -3,
                        child: const SizedBox(
                          height: 10,
                          child: Icon(Icons.arrow_drop_down,
                              size: 40, color: Colors.black),
                        ),
                      ),
                    ],
                  ),
                ),
              );
            },
          ),
        ),
        const SizedBox(height: 10),
        // Step Indicators with small vertical lines
        Padding(
          padding: const EdgeInsets.only(left: 5.0),
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: _buildStepIndicators(),
          ),
        ),
      ],
    );
    

  // Method to build step indicators with vertical lines above the text
  List<Widget> _buildStepIndicators() {
    List<Widget> indicators = [];
    for (double i = widget.min; i <= widget.max; i += widget.step) {
      indicators.add(
        Column(
          children: [
            // Vertical small line above the indicator text
            Container(
              width: 1, height: 10, color: Colors.black, // Vertical small line
            ),
            const SizedBox(height: 4), // Spacing between line and text
            // Step indicator text
            Text(i.toStringAsFixed(0)),
          ],
        ),
      );
    }
    return indicators;
  }
}
 ```
