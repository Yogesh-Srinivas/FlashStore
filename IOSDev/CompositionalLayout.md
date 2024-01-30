# Compositional Layout

### References
1. https://betterprogramming.pub/ios-13-compositional-layouts-in-collectionview-90a574b410b8

---

* Compositional layouts are a declarative kind of API that allows us to build large layouts by stitching together smaller layout groups. Compositional layouts have a hierarchy that consists of: Item, Group, Section, and Layout.

![image](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Xm6Xe0OaYmSAlUoOxQ7WTg.png)


* To build any compositional layout, the following four classes need to be implemented:

  **NSCollectionLayoutSize** — The width and height dimensions are of the type NSCollectionLayoutDimension which can be defined by setting the fractional width/height of the layout (percentage relative to its container), or by setting the absolute or estimated sizes.
  
  **NSCollectionLayoutItem** — This is your layout’s cell that renders on the screen based on the size.
  
  **NSCollectionLayoutGroup** — It holds the NSCollectionLayoutItem in either horizontal, vertical, or custom forms.
  
  **NSCollectionLayoutSection** — This is used to initialize the section by passing along the NSCollectionLayoutGroup. Sections eventually compose the compositional layouts.

