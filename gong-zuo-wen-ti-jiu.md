##### 1.SourceTree问题记录

`[user]`

`name = ioszhangyang`

`email = zhangyang@mimidai.com`

##### 2.手机分辨率

| 设备 |
| :--- |


|  | 屏幕尺寸 | 分辨率（pt） | Reader | 分辨率（px） | 渲染后 | PPI |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| iPhone 3GS | 3.5吋 | 320x480 | @1x | 320x480 |  | 163 |
| iPhone 4/4s | 3.5吋 | 320x480 | @2x | 640x960 |  | 330 |
| iPhone 5/5s/5c | 4.0吋 | 320x568 | @2x | 640x1136 |  | 326 |
| iPhone 6 | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 6Plus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |
| iPhone 6s | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 6sPlus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |
| iPhone 7 | 4.7吋 | 375x667 | @2x | 750x1334 |  | 326 |
| iPhone 7Plus | 5.5吋 | 414x736 | @3x | 1242x2208 | 1080x1920 | 401 |

##### 3.获取控件XY

CGRectGetMaxY\(View\)

**CGRectGetMinX方法的作用得到目前view在当前屏幕中相对于整个View的最小值（位于屏幕的最左边）**

**CGRectGetMaxX方法的作用得到目前view在当前屏幕中相对于整个View的最大值（位于屏幕的最右边）**

**CGRectGetMinY方法的作用得到目前view在当前屏幕中相对于整个View的最小值（位于屏幕的最上边）**

**CGRectGetMaxY方法的作用得到目前view在当前屏幕中相对于整个View的最大值（位于屏幕的最下边）**
