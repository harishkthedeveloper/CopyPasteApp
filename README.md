
```
import 'package:flutter/material.dart';

class SliderContainer extends StatelessWidget {
  const SliderContainer({
    Key? key,
    this.dragPosition = 0.0,
    required this.onChanged,
    this.min = 0.0,
    this.max = 100.0,
  }) : super(key: key);

  final double dragPosition;
  final ValueChanged<double> onChanged;
  final double min;
  final double max;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        final screenWidth = constraints.maxWidth;
        final calculatedWidth = dragPosition ;

        return Column(
          children: [
            GestureDetector(
              child: SizedBox(
                height: 50,
                child: Container(
                  decoration: BoxDecoration(
                    border: Border.all(color: Colors.grey),
                  ),
                  child: Stack(
                    children: [
                      Positioned(
                        left: 0,
                        top: 0,
                        bottom: 0,
                        width: calculatedWidth,
                        child: Container(
                          color: Colors.blue,
                        ),
                      ),
                      Positioned(
                         left: calculatedWidth, // Use calculatedWidth here
                        top: 0,
                        child: GestureDetector(
                          onHorizontalDragUpdate: (details) {
                            onChanged(dragPosition + details.delta.dx);
                          },
                          child: Container(
                            width: 20,
                            height: 50,
                            color: Colors.grey[400],
                            child: const Center(
                              child:
                                  Icon(Icons.drag_handle, color: Colors.white),
                            ),
                          ),
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ),
          ],
        );
      },
    );
  }


}

```
