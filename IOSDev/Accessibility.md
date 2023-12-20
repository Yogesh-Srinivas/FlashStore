# Accessibilty
---
### References:
- https://developer.apple.com/documentation/uikit/accessibility_for_uikit/supporting_voiceover_in_your_app
- [Audio Graph - kodeco](https://www.kodeco.com/31561694-ios-accessibility-in-swiftui-create-accessible-charts-using-audio-graphs)

---
- know that there’s a naming convention that can be used to avoid typing down a-c-c-e-s-s-i-b-i-l-i-t-y each and every time time you create a variable for a label or an identifier. This naming convention is a11y.


### Chart Coloring

> - Use hight contrast color for charts. The chartColor and backgorind color is of 4.5:1 ratio minimum.
> - Avoid using Red and Green color combinations. And also blue and yellow.
> - Use symbols in addition to color.

### Wording
> - Concern about singular and plural in vocie over.

### Navigation
> - When there are more number of dataPoints, dont navigate each element. Split the intervals and navigate reasonably.
> - 



## Audio Graphs

> **Continuous vs. Noncontinuous Data**
> - In continuous data sets, each data point connects with its predecessor and follower. Audio Graphs presents that kind of data as a line chart.
> - In noncontinuous data sets, also called discrete data sets, each data point is separated. The Audio Graph looks like a bar chart for categorical axes or shows circles for numerical axes.
> - When a view supports Audio Graphs, this appears as an option on the VoiceOver rotor. It lets up and down swipes offer the following options for a graph:
>   * Describe chart: Reads a description of the chart.
>   * Play Audio Graph: Plays sounds based on the data in the chart.
>   * Chart Details: Opens a separate screen so the user can navigate through more details.




