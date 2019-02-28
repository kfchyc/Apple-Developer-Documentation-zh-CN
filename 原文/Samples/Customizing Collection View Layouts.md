- [UIKit](dash-apple-api://load?request_key=csuikit)  
- [Views and Controls](dash-apple-api://load?request_key=ts2864393) 
- [Collection Views](dash-apple-api://load?request_key=ts2864442) 
- Customizing Collection View Layouts

Sample Code

# Customizing Collection View Layouts

Language

- Swift
- [Objective-C](dash-apple-api://load?topic_id=3021192&language=occ)

SDKs

- iOS 12.0+
- Xcode 10.0+

Customize a view layout by changing the size of cells in the flow or implementing a mosaic style.

Download

------

## Overview

To lay out UICollectionView cells in a simple grid, you can use [`UICollectionViewFlowLayout`](dash-apple-api://load?topic_id=1617715&language=swift) directly. For more flexibility, you can subclass [`UICollectionViewLayout`](dash-apple-api://load?topic_id=1617758&language=swift) to create advanced layouts.

This sample app demonstrates two custom layout subclasses:

- `ColumnFlowLayout` — A `UICollectionViewFlowLayout` subclass that arranges cells in a list format for narrow screens, or as a grid for wider screens. See “For a Simple Grid, Size Cells Dynamically,” below.
- `MosaicLayout` — A `UICollectionViewLayout` subclass that lays out cells in a mosaic-style, nonconforming grid. See “For a Complex Grid, Define Cell Sizes Explicitly,” below.

The app opens to the Friends view controller, which uses a column flow layout to display a list of people. Tapping any cell takes you to the Feed view controller, which uses a mosaic layout to display photos from the user’s photo library.

Tapping the cloud icon to the right of the navigation bar demonstrates batched animations for inserting, deleting, moving, and reloading items in the collection view. For more information, see “Perform Batch Updates,” below. Using pull-to-refresh on the collection view resets the data.

### For a Simple Grid, Size Cells Dynamically

`ColumnFlowLayout` is a subclass of [`UICollectionViewFlowLayout`](dash-apple-api://load?topic_id=1617715&language=swift) that uses the size of the collection view to determine the width of its cells. If only one cell will fit comfortably horizontally, the cells are arranged to occupy the entire width of the collection view. Otherwise, multiple columns of cells are displayed with a fixed width.

In practice, on iPhone devices in portrait mode, `ColumnFlowLayout` displays a single vertical column of cells. In landscape mode, or on an iPad, it displays a grid layout.

Use the [`prepare()`](dash-apple-api://load?topic_id=1617752&language=swift) function to compute the available screen width of the device and set the [`itemSize`](dash-apple-api://load?topic_id=1617711&language=swift) property accordingly.

```swift
override func prepare() {
    super.prepare()

    guard let collectionView = collectionView else { return }

        let availableWidth = collectionView.bounds.inset(by: collectionView.layoutMargins).width
    let maxNumColumns = Int(availableWidth / minColumnWidth)
    let cellWidth = (availableWidth / CGFloat(maxNumColumns)).rounded(.down)

        self.itemSize = CGSize(width: cellWidth, height: cellHeight)
    self.sectionInset = UIEdgeInsets(top: self.minimumInteritemSpacing, left: 0.0, bottom: 0.0, right: 0.0)
    self.sectionInsetReference = .fromSafeArea
}
```



### For a Complex Grid, Define Cell Sizes Explicitly

If you need more customization than is possible with a subclass of [`UICollectionViewFlowLayout`](dash-apple-api://load?topic_id=1617715&language=swift), subclass [`UICollectionViewLayout`](dash-apple-api://load?topic_id=1617758&language=swift) instead.

`MosaicLayout` is a `UICollectionViewLayout` subclass that displays an arbitrary number of cells with differing sizes and aspect ratios. It’s used by `FeedViewController` to display images from the user’s photo library. Cells are organized into rows in one of four styles, from a single cell to multiple cells in varying layouts.



![Images showing a row of four rectangles, each representing a mosaic style. On the left, a single cell. Second from left, two equal-size cells. Third from left, one cell occupying two-thirds of the area, and two stacked cells to the left of the larger cell. Last, one cell occupying two-thirds of the area, and two stacked cells to the right of the larger cell.](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABYwAAADyCAYAAADuv4HNAAAAAXNSR0IArs4c6QAAJ5tJREFUeAHt3T9X1coaB+B411kLO+3stKOkpIOOTks6S7+S38BOOu0o6aCzkw46O+ik4vrm3tkrhD2ejYaQ5H2ylifZs/NvnkmG8DsheXZ7e9v8f1hNlAJjAgQIJBN4NpP66q9n0lB2kwCBRxOYS39dAPTbRcKYAIGsAlPvt/XTWY9M9SZAoC/Q9tf/6Zf6TIAAAQIECBAgQIAAAQIECBAgQIAAAQI5BQTGOdtdrQkQIECAAAECBAgQIECAAAECBAgQIHBPQGB8j0QBAQIECBAgQIAAAQIECBAgQIAAAQIEcgr8U6v2+fl57SvlBAgQWITA9vb2Iuqhv15EM86mEv3zxvE3m6ab9Y72j7tZV+bXzjtv5t6C89j//nnjuJtHuy1lL/vH39zq5XyZW4vNe3/754vjb97tObe97x9/Zf/dYVwkjAkQIECAAAECBAgQIECAAAECBAgQIJBcQGCc/ABQfQIECBAgQIAAAQIECBAgQIAAAQIECBQBgXGRMCZAgAABAgQIECBAgAABAgQIECBAgEByAYFx8gNA9QkQIECAAAECBAgQIECAAAECBAgQIFAEBMZFwpgAAQIECBAgQIAAAQIECBAgQIAAAQLJBQTGyQ8A1SdAgAABAgQIECBAgAABAgQIECBAgEAREBgXCWMCBAgQIECAAAECBAgQIECAAAECBAgkFxAYJz8AVJ8AAQIECBAgQIAAAQIECBAgQIAAAQJFQGBcJIwJECBAgAABAgQIECBAgAABAgQIECCQXEBgnPwAUH0CBAgQIECAAAECBAgQIECAAAECBAgUAYFxkTAmQIAAAQIECBAgQIAAAQIECBAgQIBAcgGBcfIDQPUJECBAgAABAgQIECBAgAABAgQIECBQBATGRcKYAAECBAgQIECAAAECBAgQIECAAAECyQUExskPANUnQIAAAQIECBAgQIAAAQIECBAgQIBAERAYFwljAgQIECBAgAABAgQIECBAgAABAgQIJBcQGCc/AFSfAAECBAgQIECAAAECBAgQIECAAAECRUBgXCSMCRAgQIAAAQIECBAgQIAAAQIECBAgkFxAYJz8AFB9AgQIECBAgAABAgQIECBAgAABAgQIFAGBcZEwJkCAAAECBAgQIECAAAECBAgQIECAQHIBgXHyA0D1CRAgQIAAAQIECBAgQIAAAQIECBAgUAQExkXCmAABAgQIECBAgAABAgQIECBAgAABAskFBMbJDwDVJ0CAAAECBAgQIECAAAECBAgQIECAQBEQGBcJYwIECBAgQIAAAQIECBAgQIAAAQIECCQXEBgnPwBUnwABAgQIECBAgAABAgQIECBAgAABAkVAYFwkjAkQIECAAAECBAgQIECAAAECBAgQIJBcQGCc/ABQfQIECBAgQIAAAQIECBAgQIAAAQIECBQBgXGRMCZAgAABAgQIECBAgAABAgQIECBAgEByAYFx8gNA9QkQIECAAAECBAgQIECAAAECBAgQIFAEBMZFwpgAAQIECBAgQIAAAQIECBAgQIAAAQLJBQTGyQ8A1SdAgAABAgQIECBAgAABAgQIECBAgEAREBgXCWMCBAgQIECAAAECBAgQIECAAAECBAgkFxAYJz8AVJ8AAQIECBAgQIAAAQIECBAgQIAAAQJFQGBcJIwJECBAgAABAgQIECBAgAABAgQIECCQXEBgnPwAUH0CBAgQIECAAAECBAgQIECAAAECBAgUAYFxkTAmQIAAAQIECBAgQIAAAQIECBAgQIBAcgGBcfIDQPUJECBAgAABAgQIECBAgAABAgQIECBQBATGRcKYAAECBAgQIECAAAECBAgQIECAAAECyQUExskPANUnQIAAAQIECBAgQIAAAQIECBAgQIBAERAYFwljAgQIECBAgAABAgQIECBAgAABAgQIJBcQGCc/AFSfAAECBAgQIECAAAECBAgQIECAAAECRUBgXCSMCRAgQIAAAQIECBAgQIAAAQIECBAgkFxAYJz8AFB9AgQIECBAgAABAgQIECBAgAABAgQIFAGBcZEwJkCAAAECBAgQIECAAAECBAgQIECAQHIBgXHyA0D1CRAgQIAAAQIECBAgQIAAAQIECBAgUAQExkXCmAABAgQIECBAgAABAgQIECBAgAABAskFBMbJDwDVJ0CAAAECBAgQIECAAAECBAgQIECAQBEQGBcJYwIECBAgQIAAAQIECBAgQIAAAQIECCQXEBgnPwBUnwABAgQIECBAgAABAgQIECBAgAABAkVAYFwkjAkQIECAAAECBAgQIECAAAECBAgQIJBcQGCc/ABQfQIECBAgQIAAAQIECBAgQIAAAQIECBQBgXGRMCZAgAABAgQIECBAgAABAgQIECBAgEByAYFx8gNA9QkQIECAAAECBAgQIECAAAECBAgQIFAEBMZFwpgAAQIECBAgQIAAAQIECBAgQIAAAQLJBQTGyQ8A1SdAgAABAgQIECBAgAABAgQIECBAgEAREBgXCWMCBAgQIECAAAECBAgQIECAAAECBAgkFxAYJz8AVJ8AAQIECBAgQIAAAQIECBAgQIAAAQJFQGBcJIwJECBAgAABAgQIECBAgAABAgQIECCQXOCf5PVXfQIECBAgQIAAAQIEnkDg58+fzdnZWbvlvb291R50y1eFnYnXr183b9686ZSsnzw/P2++f//e/PjxYzXDq1evmp2dnY2WXy1kggABAgQIEBhNoHsd4PpgNPZ7GxIY3yNRQIAAAQIECBAgQIDAYwtcX183p6enzYsXL5ruL4Q3NzdteW37ZZnDw8N22f588Yvm169fm4uLi9VXz58/b6I8thlBcgTOb9++baLcQIAAAQIECExHwPXBNNpCYDyNdrAXBAgQIECAAAECBAh0BCLMPTg46JQ0bej77du39q7hz58/N+/fv78X+h4dHbXfRxC9v7/fxB3Jsa74BTRC5AicYxzzxfIGAgQIECBAYD4Crg/GaSuB8TjOtkKAAAECBAgQIECAwAMEtra2mu3t7XtLxCMlPn361IbCER7v7u6u5jk5OVmFxf0wOQLk8jiKCJvjURWxfJQZCBAgQIAAgXkIuD4Yp5289G4cZ1shQIAAAQIECBAgQGAggRLyXl5ertYYj5yIADiGuDO59riJCI5LyFzmX63EBAECBAgQIDBbAdcHwzWdO4yHs7QmAgQIECBAgAABAgRGEChhcITEZYjwOD7Hi+3+7aV48QtlzGcgQIAAAQIEliPg+mC4thQYD2dpTQQIECBAgAABAgQIjCBQguLyi2FsMh4xEcOmQfCm87Ur9R8CBAgQIEBg8gKuD4ZrIo+kGM7SmggQIECAAAECBAgQGEGgPEoiXmhXhqurq3YyHjlhIECAAAECBPIJuD4Yrs3dYTycpTURIECAAAECBAgQIDCQwM3NTXN+fn5nbREKxy+D19fXTXmJ3Z0ZKh8uLi6a4+Pjtd8eHh6261r7pUICBAgQIEBgUgKuD8ZpDoHxOM62QoAAAQIECBAgQIDAAwTiz0q/fPmydol4nMS7d+/uvNiu+3iKtQv1CiN0NhAgQIAAAQLzEnB9ME57CYzHcbYVAgQIECBAgAABAgQeKLC7u3tnia2trepL7cqjKMqzjLsLxkvwPnz4sCqKXzY/fvy4+myCAAECBAgQmI+A64PHbyuB8eMb2wIBAgQIECBAgAABAg8UiAB4b29v46UiFD45OWkuLy+bCIR/d8dxubs4tlGC5o03ZEYCBAgQIEDgyQRcH4xD76V34zjbCgECBAgQIECAAIF0AuXPRtc9WiK+G3KIx1TEv1jv2dnZb1ddXooTIbOBAAECBAgQGFfA9cG43n+yNYHxn6hZhgABAgQIECBAgACBewKnp6ftc4fLHbwxQ9zxGy+vixfPdYfyQrsIeYcayh3JsR8lFO6vu/td/09a+/P6TIAAAQIECPy9gOuDvzccew0eSTG2uO0RIECAAAECBAgQWKBAhMTxSIgylJfS7ezsNPGL4tHRURPT8aekESKXAHnI0DbuGI7QOPbj+Pi4DY0jkI5txv7FNkuYHfvncRSltYwJECBAgMDjCLg+eBzXx16rwPixha2fAAECBAgQIECAQAKBCF9LMNu9a7h21288Y/jg4KB9jMSQPBFAx/YjMI4X4PVfghffxT55HMWQ6tZFgAABAgTWC7g+WO8y9dJnt7e3ZR9XE1FQ/kSsfGlMgACBpQlsb2/3q/SsXzDRz/rriTZMht3qnzeuFzK0+tPXsX/c/dqjufTXBS9Vvx13Eq27czfKI7y9urpqA90Ibmsvpit3Aa9bT0HdZFwC43hW4suXL9vt/u06N9nuFObpnzf66ym0Sp596B9/v2o+9X47VT+d50icR03758tS+2vXB9M8HvvHX+mv3WE8zfayVwQIECBAgAABAgRmKVALZKO89l2/opvO11+u/zlC6fhnIECAAAECBJ5WoPaz3fXB07ZLbeteeleTUU6AAAECBAgQIECAAAECBAgQIECAAIFkAgLjZA2uugQIECBAgAABAgQIECBAgAABAgQIEKgJCIxrMsoJECBAgAABAgQIECBAgAABAgQIECCQTEBgnKzBVZcAAQIECBAgQIAAAQIECBAgQIAAAQI1AYFxTUY5AQIECBAgQIAAAQIECBAgQIAAAQIEkgkIjJM1uOoSIECAAAECBAgQIECAAAECBAgQIECgJiAwrskoJ0CAAAECBAgQIECAAAECBAgQIECAQDIBgXGyBlddAgQIECBAgAABAgQIECBAgAABAgQI1AQExjUZ5QQIECBAgAABAgQIECBAgAABAgQIEEgmIDBO1uCqS4AAAQIECBAgQIAAAQIECBAgQIAAgZqAwLgmo5wAAQIECBAgQIAAAQIECBAgQIAAAQLJBATGyRpcdQkQIECAAAECBAgQIECAAAECBAgQIFATEBjXZJQTIECAAAECBAgQIECAAAECBAgQIEAgmYDAOFmDqy4BAgQIECBAgAABAgQIECBAgAABAgRqAgLjmoxyAgQIECBAgAABAgQIECBAgAABAgQIJBMQGCdrcNUlQIAAAQIECBAgQIAAAQIECBAgQIBATUBgXJNRToAAAQIECBAgQIAAAQIECBAgQIAAgWQCAuNkDa66BAgQIECAAAECBAgQIECAAAECBAgQqAkIjGsyygkQIECAAAECBAgQIECAAAECBAgQIJBMQGCcrMFVlwABAgQIECBAgAABAgQIECBAgAABAjUBgXFNRjkBAgQIECBAgAABAgQIECBAgAABAgSSCQiMkzW46hIgQIAAAQIECBAgQIAAAQIECBAgQKAmIDCuySgnQIAAAQIECBAgQIAAAQIECBAgQIBAMgGBcbIGV10CBAgQIECAAAECBAgQIECAAAECBAjUBATGNRnlBAgQIECAAAECBAgQIECAAAECBAgQSCYgME7W4KpLgAABAgQIECBAgAABAgQIECBAgACBmoDAuCajnAABAgQIECBAgAABAgQIECBAgAABAskEBMbJGlx1CRAgQIAAAQIECBAgQIAAAQIECBAgUBMQGNdklBMgQIAAAQIECBAgQIAAAQIECBAgQCCZwD/J6qu6Iwj8/PmzOTs7a7e0t7e32mK3fFXYmXj9+nXz5s2bTsn6yfPz8+b79+/Njx8/VjO8evWq2dnZ2Wj51UImCBBYjEC3f9HvLKZZJ18Rx93km8gOEiBAoBXQXzsQCPy5gPPnz+0sSWDOAgLjObfeRPf9+vq6OT09bV68eNF0g5ubm5u2vLbbZZnDw8N22f588YPq69evzcXFxeqr58+fN1Ee24wgOQLnt2/fNlFuIEAgj4B+J09bT6mmjrsptYZ9IUCAQF1Af1238Q2BfxNw/vybkO8JLFNAYLzMdp10rSLMPTg4uLOPEfp++/atvWv48+fPzfv37++FvkdHR+33EUTv7+83cUdyrCt+gEWI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CBAgQIAAAQIECCQWEBgnbnxVJ0CAAAECBAgQIECAAAECBAgQIECAQFdAYNzVME2AAAECBAgQIECAAAECBAgQIECAAIHEAgLjxI2v6gQIECBAgAABAgQIECBAgAABAgQIEOgKCIy7GqYJECBAgAABAgQIECBAgAABAgQIECCQWEBgnLjxVZ0AAQIECBAgQIAAAQIECBAgQIAAAQJdAYFxV8M0AQIECBAgQIAAAQIECBAgQIAAAQIEEgsIjBM3vqoTIECAAAECBAgQIECAAAECBAgQIECgKyAw7mqYJkCAAAECBAgQIECAAAECBAgQIECAQGIBgXHixld1AgQIECBAgAABAgQIECBAgAABAgQIdAUExl0N0wQIECBAgAABAgQIECBAgAABAgQIEEgsIDBO3PiqToAAAQIECBAgQIAAAQIECBAgQIAAga6AwLirYZoAAQIECBAgQIAAAQIECBAgQIAAAQKJBQTGiRtf1QkQIECAAAECBAgQIECAAAECBAgQINAVEBh3NUwTIECAAAECBAgQIECAAAECBAgQIEAgsYDAOHHjqzoBAgQIECBAgAABAgQIECBAgAABAgS6AgLjroZpAgQIECBAgAABAgQIECBAgAABAgQIJBYQGCdufFUnQIAAAQIECBAgQIAAAQIECBAgQIBAV0Bg3NUwTYAAAQIECBAgQIAAAQIECBAgQIAAgcQCAuPEja/qBAgQIECAAAECBAgQIECAAAECBAgQ6AoIjLsapgkQIECAAAECBAgQIECAAAECBAgQIJBYQGCcuPFVnQABAgQIECBAgAABAgQIECBAgAABAl0BgXFXwzQBAgQIECBAgAABAgQIECBAgAABAgQSCwiMEze+qhMgQIAAAQIECBAgQIAAAQIECBAgQKArIDDuapgmQIAAAQIECBAgQIAAAQIECBAgQIBAYgGBceLGV3UCBAgQIECAAAECBAgQIECAAAECBAh0BQTGXQ3TBAgQIECAAAECBAgQIECAAAECBAgQSCwgME7c+KpOgAABAgQIECBAgAABAgQIECBAgACBroDAuKthmgABAgQIECBAgAABAgQIECBAgAABAokFBMaJG1/VCRAgQIAAAQIECBAgQIAAAQIECBAg0BX4p/vBNAECBAgQIECAAAECBMYQ+PnzZ3N2dtZuam9vb7XJbvmqsDPx+vXr5s2bN52S9ZPn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qAkIjGsyygkQIECAAAECBAgQIECAAAECBAgQIJBMQGCcrMFVlwABAgQIECBAgAABAgQIECBAgAABAjUBgXFNRjkBAgQIECBAgAABAgQIECBAgAABAgSSCQiMkzW46hIgQIAAAQIECBAgQIAAAQIECBAgQKAmIDCuySgnQIAAAQIECBAgQIAAAQIECBAgQIBAMgGBcbIGV10CBAgQIECAAAECBAgQIECAAAECBAjUBATGNRnlBAgQIECAAAECBAgQIECAAAECBAgQSCYgME7W4KpLgAABAgQIECBAgAABAgQIECBAgACBmoDAuCajnAABAgQIECBAgAABAgQIECBAgAABAskEBMbJGlx1CRAgQIAAAQIECBAgQIAAAQIECBAgUBMQGNdklBMgQIAAAQIECBAgQIAAAQIECBAgQCCZgMA4WYOrLgECBAgQIECAAAECBAgQIECAAAECBGoCAuOajHICBAgQIECAAAECBAgQIECAAAECBAgkExAYJ2tw1SVAgAABAgQIECBAgAABAgQIECBAgEBNQGBck1FOgAABAgQIECBAgAABAgQIECBAgACBZAIC42QNrroECBAgQIAAAQIECBAgQIAAAQIECBCoCQiMazLKCRAgQIAAAQIECBAgQIAAAQIECBAgkExAYJyswVWXAAECBAgQIECAAAECBAgQIECAAAECNQGBcU1GOQECBAgQIECAAAECBAgQIECAAAECBJIJCIyTNbjqEiBAgAABAgQIECBAgAABAgQIECBAoCYgMK7JKCdAgAABAgQIECBAgAABAgQIECBAgEAyAYFxsgZXXQIECBAgQIAAAQIECBAgQIAAAQIECNQEBMY1GeUECBAgQIAAAQIECBAgQIAAAQIECBBIJiAwTtbgqkuAAAECBAgQIECAAAECBAgQIECAAIGagMC4JqOcAAECBAgQIECAAAECBAgQIECAAAECyQQExskaXHUJECBAgAABAgQIECBAgAABAgQIECBQExAY12SUEyBAgAABAgQIECBAgAABAgQIECBAIJmAwDhZg6suAQIECBAgQIAAAQIECBAgQIAAAQIEagIC45qMcgIECBAgQIAAAQIECBAgQIAAAQIECCQTEBgna3DVJUCAAAECBAgQIECAAAECBAgQIECAQE1AYFyTUU6AAAECBAgQIECAAAECBAgQIECAAIFkAgLjZA2uugQIECBAgAABAgQIECBAgAABAgQIEKgJCIxrMsoJECBAgAABAgQIECBAgAABAgQIECCQTEBgnKzBVZcAAQIECBAgQIAAAQIECBAgQIAAAQI1AYFxTUY5AQIECBAgQIAAAQIECBAgQIAAAQIEkgkIjJM1uOoSIECAAAECBAgQIECAAAECBAgQIECgJvDs9va2fLeaKAXGBAgQSCbwbCb11V/PpKHsJgECjyYwl/66AOi3i4QxAQJZBabeb+unsx6Z6k2AQF+g7a/dYdxn8ZkAAQIECBAgQIAAAQIECBAgQIAAAQJJBQTGSRtetQkQIECAAAECBAgQIECAAAECBAgQINAXEBj3RXwmQIAAAQIECBAgQIAAAQIECBAgQIBAUoH/ApUl6eYSr/IxAAAAAElFTkSuQmCC)



**Calculate Cell Dimensions**

The [`prepare()`](dash-apple-api://load?topic_id=1617752&language=swift) method is called whenever a layout is invalidated. Override this method to calculate the position and size of every cell, as well as the total dimensions for the entire layout.

```swift
override func prepare() {
    super.prepare()

        guard let collectionView = collectionView else { return }

    // Reset cached information.
    cachedAttributes.removeAll()
    contentBounds = CGRect(origin: .zero, size: collectionView.bounds.size)

        // For every item in the collection view:
    //  - Prepare the attributes.
    //  - Store attributes in the cachedAttributes array.
    //  - Combine contentBounds with attributes.frame.
    let count = collectionView.numberOfItems(inSection: 0)

        var currentIndex = 0
    var segment: MosaicSegmentStyle = .fullWidth
    var lastFrame: CGRect = .zero

        let cvWidth = collectionView.bounds.size.width

        while currentIndex < count {
        let segmentFrame = CGRect(x: 0, y: lastFrame.maxY + 1.0, width: cvWidth, height: 200.0)

                var segmentRects = [CGRect]()
        switch segment {
        case .fullWidth:
            segmentRects = [segmentFrame]

                    case .fiftyFifty:
            let horizontalSlices = segmentFrame.dividedIntegral(fraction: 0.5, from: .minXEdge)
            segmentRects = [horizontalSlices.first, horizontalSlices.second]

                    case .twoThirdsOneThird:
            let horizontalSlices = segmentFrame.dividedIntegral(fraction: (2.0 / 3.0), from: .minXEdge)
            let verticalSlices = horizontalSlices.second.dividedIntegral(fraction: 0.5, from: .minYEdge)
            segmentRects = [horizontalSlices.first, verticalSlices.first, verticalSlices.second]

                    case .oneThirdTwoThirds:
            let horizontalSlices = segmentFrame.dividedIntegral(fraction: (1.0 / 3.0), from: .minXEdge)
            let verticalSlices = horizontalSlices.first.dividedIntegral(fraction: 0.5, from: .minYEdge)
            segmentRects = [verticalSlices.first, verticalSlices.second, horizontalSlices.second]
        }

                // Create and cache layout attributes for calculated frames.
        for rect in segmentRects {
            let attributes = UICollectionViewLayoutAttributes(forCellWith: IndexPath(item: currentIndex, section: 0))
            attributes.frame = rect

                        cachedAttributes.append(attributes)
            contentBounds = contentBounds.union(lastFrame)

                        currentIndex += 1
            lastFrame = rect
        }

        // Determine the next segment style.
        switch count - currentIndex {
        case 1:
            segment = .fullWidth
        case 2:
            segment = .fiftyFifty
        default:
            switch segment {
            case .fullWidth:
                segment = .fiftyFifty
            case .fiftyFifty:
                segment = .twoThirdsOneThird
            case .twoThirdsOneThird:
                segment = .oneThirdTwoThirds
            case .oneThirdTwoThirds:
                segment = .fiftyFifty
            }
        }
    }
}
```



**Provide the Content Size**

Override the [`collectionViewContentSize`](dash-apple-api://load?topic_id=1617796&language=swift) property, providing a size for the collection view.

```swift
override var collectionViewContentSize: CGSize {
    return contentBounds.size
}
```



**Define the Layout Attributes**

Override [`layoutAttributesForElements(in:)`](dash-apple-api://load?topic_id=1617769&language=swift), defining the layout attributes for a geometric region. The collection view calls this function periodically to display items, which is known as *querying by geometric region*.

```swift
override func layoutAttributesForElements(in rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
    var attributesArray = [UICollectionViewLayoutAttributes]()

        // Find any cell that sits within the query rect.
    guard let lastIndex = cachedAttributes.indices.last,
          let firstMatchIndex = binSearch(rect, start: 0, end: lastIndex) else { return attributesArray }

        // Starting from the match, loop up and down through the array until all the attributes
    // have been added within the query rect.
    for attributes in cachedAttributes[..<firstMatchIndex].reversed() {
        guard attributes.frame.maxY >= rect.minY else { break }
        attributesArray.append(attributes)
    }

        for attributes in cachedAttributes[firstMatchIndex...] {
        guard attributes.frame.minY <= rect.maxY else { break }
        attributesArray.append(attributes)
    }

        return attributesArray
}
```



Also provide the layout attributes for a specific item by implementing [`layoutAttributesForItem(at:)`](dash-apple-api://load?topic_id=1617797&language=swift). The collection view calls this function periodically to display one particular item, which is known as *querying by index path*.

```swift
override func layoutAttributesForItem(at indexPath: IndexPath) -> UICollectionViewLayoutAttributes? {
    return cachedAttributes[indexPath.item]
}
```



Because these functions are called often, they can affect the performance of your app. To make them as efficient as possible, follow the example code as closely as you can.

**Handle Bounds Changes**

The [`shouldInvalidateLayout(forBoundsChange:)`](dash-apple-api://load?topic_id=1531047&language=swift) function is called for every bounds change from the collection view, or whenever its size or origin changes. This function is also called frequently during scrolling. The default implementation returns `false`, or, if the size and origin change, it returns `true`.

```swift
override func shouldInvalidateLayout(forBoundsChange newBounds: CGRect) -> Bool {
    guard let collectionView = collectionView else { return false }
    return !newBounds.size.equalTo(collectionView.bounds.size)
}
```



For optimum performance, this sample performs a binary search inside [`layoutAttributesForElements(in:)`](dash-apple-api://load?topic_id=1617769&language=swift)instead of a linear search of the attributes it needs for each element in a given bounds area.

### Perform Batch Updates

Tapping the top-right button in the navigation bar triggers the collection view to perform a *batch update* of multiple animated operations (insert, delete, move, and reload) of its collection view cells all at the same time.

Within a call to [`performBatchUpdates(_:completion:)`](dash-apple-api://load?topic_id=1618045&language=swift), all insert, delete, move, and reload operations are animated simultaneously. In this sample, batch updates are made by processing an array of `PersonUpdate` objects, each of which encapsulates one update:

- `insert` with a `Person` object and insertion index.
- `delete` with an index.
- `move` from one index to another.
- `reload` with an index.

First, the `reload` operations are performed without animation because no cell movement is involved:

```swift
// Perform any cell reloads without animation because there is no movement.
UIView.performWithoutAnimation {
    collectionView.performBatchUpdates({
        for update in remoteUpdates {
            if case let .reload(index) = update {
                people[index].isUpdated = true
                collectionView.reloadItems(at: [IndexPath(item: index, section: 0)])
            }
        }
    })
}
```



Next, the remaining operations are animated:

```swift
// Animate all other update types together.
collectionView.performBatchUpdates({
    var deletes = [Int]()
    var inserts = [(person:Person, index:Int)]()

    for update in remoteUpdates {
        switch update {
        case let .delete(index):
            collectionView.deleteItems(at: [IndexPath(item: index, section: 0)])
            deletes.append(index)

                    case let .insert(person, index):
            collectionView.insertItems(at: [IndexPath(item: index, section: 0)])
            inserts.append((person, index))

                    case let .move(fromIndex, toIndex):
            // Updates that move a person are split into an addition and a deletion.
            collectionView.moveItem(at: IndexPath(item: fromIndex, section: 0),
                                    to: IndexPath(item: toIndex, section: 0))
            deletes.append(fromIndex)
            inserts.append((people[fromIndex], toIndex))

                    default: break
        }
    }

        // Apply deletions in descending order.
    for deletedIndex in deletes.sorted().reversed() {
        people.remove(at: deletedIndex)
    }

        // Apply insertions in ascending order.
    let sortedInserts = inserts.sorted(by: { (personA, personB) -> Bool in
        return personA.index <= personB.index
    })
    for insertion in sortedInserts {
        people.insert(insertion.person, at: insertion.index)
    }

        // The update button is enabled only if the list still has people in it.
    navigationItem.rightBarButtonItem?.isEnabled = !people.isEmpty
})
```