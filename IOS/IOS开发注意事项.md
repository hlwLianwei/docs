# swfit 中 UIScrollView 不滚动的问题

- 设置 UIScrollView 的约束为上下左右都为 0;
- 在 UIScrollView 添加一个 UIImageView 的控件，设置 UIImageView 的约束为上下左右都为 0，再设置其 Intrinsic Size 为 Placeholder;
- 在 UIScrollView 添加一个 UIView 的控件，设置 UIView 的约束为上左右都为 0，约束的高度为自定义高度，然后在 UIView 上添加我们需要设计的界面控件;
- 在 swift 代码中，定义一个 UIScrollView 类型变量 mSv, 并且与 Storyboard 上的 UIScrollView 相关联，然后代码设置 mSv.contentSize 为 UIView 控件的实际大小(一般宽度为屏幕宽度，高度为实现 UIView 的高度)。
