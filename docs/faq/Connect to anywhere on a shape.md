# Connect to anywhere on a shape

Normally, the diagram editor switches between connection behaviour automatically:

- Hover over or near a connection point (a cross highlighted with a green circle) while drawing a connector and it will snap to that point.
- Hover over the inside of the shape and it will connect to the shape outline (the shape outline will be highlighted in blue).
- Hover over the outline or between the outline and its perimeter (the shape outline will be highlighted in green).

## Attach a connector to any location on a shape

You can force a floating or fixed connection at any location by using a keyboard shortcut, even if the shape has [custom connection points](https://www.diagrams.net/doc/faq/shape-connection-points-customise.html) or the [*snap to point* shape property](https://www.diagrams.net/doc/faq/snap-to-point.html) enabled.

1. To add a fixed connection to anywhere within a shape, hold down `Alt` as you drag the connector into position (green shape outline).
   The connector then ignores all of the defined connection points, even when the *snap to point* shape property is set, and remain attached to the position where you attached the connector.
   ![Hold down Alt key as you connect to a shape to connect to any position on that shape](https://www.diagrams.net/assets/img/blog/connect-to-shapes-anywhere.gif)
2. To add a floating connection to the outline of the shape, ignoring the defined connection points (even with snap to point enabled), hold down `Shift` while you attach the connector to the target shape (blue shape outline and connection points are hidden).