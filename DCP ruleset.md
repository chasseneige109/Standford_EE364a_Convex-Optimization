
# ✅ Table

| **Outer function**          | **Allowed input** | **Resulting function** | **Example**     | **Valid?** |
| --------------------------- | ----------------- | ---------------------- | --------------- | ---------- |
| **Affine**                  | Anything          | Same as input          | `2*x + y`       | ✅          |
| **Convex**                  | Affine only       | Convex                 | `square(x+y)`   | ✅          |
| **Convex & nondecreasing**  | Convex input      | Convex                 | `exp(norm(x))`  | ✅          |
| **Convex & nonincreasing**  | Concave input     | Convex                 | `-log(y)` (y>0) | ✅          |
| **Concave**                 | Affine only       | Concave                | `sqrt(x+y)`     | ✅          |
| **Concave & nondecreasing** | Concave input     | Concave                | `log(x)`        | ✅          |
| **Concave & nonincreasing** | Convex input      | Concave                | `-sqrt(x)`      | ✅          |
# DCP Rule Quick Summary

| Functions                                 | Property                | Allowed Input             |
| ----------------------------------------- | ----------------------- | ------------------------- |
| `square`, `norm`, `abs`, `max`, `min`     | Convex but not monotone | **Affine only**           |
| `exp`, `-log`, `quad_over_lin`, `inv_pos` | Convex & monotone       | **Convex input allowed**  |
| `log`, `sqrt`, `geomean`                  | Concave & increasing    | **Concave input allowed** |

# ✅ Some Special Rules & Cases

when f(X) is convex,
1. square(f(X)) is convex when f(X) >= 0
2. 



# ✅ Examples from Stephan-Boyd's Lecture

## followings are convex constraints, 
## but violating CVX Rule

| Case | Original Constraint                     | Why Invalid in CVX                                             | Equivalent CVX-valid Reformulation                                          |
| ---- | --------------------------------------- | -------------------------------------------------------------- | --------------------------------------------------------------------------- |
| (a)  | `norm([x+2*y, x-y]) == 0`               | Equality with convex function (only affine allowed)            | `x + 2*y == 0`, `x - y == 0`                                                |
| (b)  | `square(square(x+y)) <= x - y`          | `square` is convex but not monotone → cannot take convex input | Introduce `t`: `square(x+y) <= t`, `square(t) <= x - y`                     |
| (c)  | `1/x + 1/y <= 1, x>=0, y>=0`            | `1/x` not DCP-valid                                            | Use `inv_pos(x) + inv_pos(y) <= 1`                                          |
| (d)  | `norm([max(x,1), max(y,2)]) <= 3*x + y` | `norm` is convex but not affine-monotone                       | Aux vars `u,v`: `max(x,1)<=u`, `max(y,2)<=v`, then `norm([u,v]) <= 3*x + y` |
| (e)  | `x*y >= 1, x>=0, y>=0`                  | Product is bilinear, not convex/concave                        | Use LMI: `[x 1; 1 y] ⪰ 0` or `geomean([x,y]) >= 1`                          |
| (f)  | `(x+y)^2 / sqrt(y) <= x - y + 5`        | Ratio of convex/concave not DCP                                | Use `quad_over_lin(x+y, sqrt(y)) <= x - y + 5`                              |
| (g)  | `x^3 + y^3 <= 1, x>=0, y>=0`            | `x^3` not convex on ℝ                                          | Use `pow_pos(x,3) + pow_pos(y,3) <= 1`                                      |
| (h)  | `x+z <= 1 + sqrt(x*y - z^2)`            | `sqrt(xy - z^2)` not DCP-valid                                 | Reform: `x+z <= 1 + geomean([ x - quad_over_lin(z,y), y ])`                 |

