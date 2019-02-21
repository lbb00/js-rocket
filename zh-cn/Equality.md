# == 和 ===

> - `===` 严格相等，会比较两个值的类型和值
> - `==` 抽象相等，比较时，会先进行类型转换，然后再比较值

参考：

- [js 中 == 和 === 的区别](https://juejin.im/entry/584918612f301e005716add6)

## 规则

以下规则涉及到几个 es 定义的抽象操作：

- Type(x) : 获取 x 的类型
- ToNumber(x) : 将 x 转换为 Number 类型
- ToBoolean(x) : 将 x 转换为 Boolean 类型
- ToString(x) : 将 x 转换为 String 类型
- SameValueNonNumber(x,y) : 计算非数字类型 x,y 是否相同
- ToPrimitive(x) : 将 x 转换为原始值

### x === y

> The comparison x === y, where x and y are values, produces true or false. Such a comparison is performed as follows:
>
> 1. If Type(x) is different from Type(y), return false.
> 2. If Type(x) is Number, then
>    - a. If x is NaN, return false.
>    - b. If y is NaN, return false.
>    - c. If x is the same Number value as y, return true.
>    - d. If x is `+0` and y is `‐0`,return true.
>    - e. If x is ‐0 and y is +0, return true.
>    - f. Return false.
> 3. Return SameValueNonNumber(x, y).
>    **NOTE** This algorithm differs from the SameValue Algorithm in its treatment of signed zeroes and NaNs.

### x == y

> The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:
>
> 1. If Type(x) is the same as Type(y), then
>    a. Return the result of performing Strict Equality Comparison x === y.
> 2. If x is null and y is undefined, return true.
> 3. If x is undefined and y is null, return true.
> 4. If Type(x) is Number and Type(y) is String, return the result of the comparison x == ToNumber(y).
> 5. If Type(x) is String and Type(y) is Number, return the result of the comparison ToNumber(x) == y.
> 6. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
> 7. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
> 8. If Type(x) is either String, Number, or Symbol and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
> 9. If Type(x) is Object and Type(y) is either String, Number, or Symbol, return the result of the comparison ToPrimitive(x)== y.
> 10. Return false.
