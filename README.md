<!-- Code generated by gomarkdoc. DO NOT EDIT -->

# genfuncs

```go
import "github.com/nwillc/genfuncs"
```

Package genfuncs implements various functions utilizing Go's Generics to help avoid writing boilerplate code\, in particular when working with slices\. Many of the functions are based on Kotlin's Sequence\.

This package\, though usable\, is primarily a proof\-of\-concept since it is likely Go will provide similar at some point soon\.

## Index

- [func All[T any](slice []T, predicate Predicate[T]) bool](<#func-all>)
- [func Any[T any](slice []T, predicate Predicate[T]) bool](<#func-any>)
- [func Associate[T any, K comparable, V any](slice []T, transform TransformKV[T, K, V]) map[K]V](<#func-associate>)
- [func AssociateWith[K comparable, V any](slice []K, valueSelector ValueSelector[K, V]) map[K]V](<#func-associatewith>)
- [func Contains[T comparable](slice []T, element T) bool](<#func-contains>)
- [func Distinct[T comparable](slice []T) []T](<#func-distinct>)
- [func Filter[T any](slice []T, predicate Predicate[T]) []T](<#func-filter>)
- [func Find[T any](slice []T, predicate Predicate[T]) (T, bool)](<#func-find>)
- [func FindLast[T any](slice []T, predicate Predicate[T]) (T, bool)](<#func-findlast>)
- [func FlatMap[T, R any](slice []T, transform Transform[T, []R]) []R](<#func-flatmap>)
- [func Fold[T, R any](slice []T, initial R, operation Operation[T, R]) R](<#func-fold>)
- [func GroupBy[T any, K comparable](slice []T, keySelector KeySelector[T, K]) map[K][]T](<#func-groupby>)
- [func JoinToString[T any](slice []T, stringer Stringer[T], separator string, prefix string, postfix string) string](<#func-jointostring>)
- [func Map[T, R any](slice []T, transform Transform[T, R]) []R](<#func-map>)
- [type KeySelector](<#type-keyselector>)
- [type Operation](<#type-operation>)
- [type Predicate](<#type-predicate>)
- [type Stringer](<#type-stringer>)
- [type Transform](<#type-transform>)
- [type TransformKV](<#type-transformkv>)
- [type ValueSelector](<#type-valueselector>)


## func All

```go
func All[T any](slice []T, predicate Predicate[T]) bool
```

All returns true if all elements of slice match the predicate\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	numbers := []float32{1, 2.2, 3.0, 4}
	positive := func(i float32) bool { return i > 0 }
	fmt.Println(genfuncs.All(numbers, positive)) // true
}
```

</p>
</details>

## func Any

```go
func Any[T any](slice []T, predicate Predicate[T]) bool
```

Any returns true if any element of the slice matches the predicate\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	fruits := []string{"apple", "banana", "grape"}
	isApple := func(fruit string) bool { return fruit == "apple" }
	isPear := func(fruit string) bool { return fruit == "pear" }
	fmt.Println(genfuncs.Any(fruits, isApple)) // true
	fmt.Println(genfuncs.Any(fruits, isPear))  // false
}
```

</p>
</details>

## func Associate

```go
func Associate[T any, K comparable, V any](slice []T, transform TransformKV[T, K, V]) map[K]V
```

Associate returns a map containing key/values provided by transform function applied to elements of the slice\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
	"strings"
)

func main() {
	byLastName := func(n string) (string, string) {
		parts := strings.Split(n, " ")
		return parts[1], n
	}
	names := []string{"fred flintstone", "barney rubble"}
	nameMap := genfuncs.Associate(names, byLastName)
	fmt.Println(nameMap["rubble"]) // barney rubble
}
```

</p>
</details>

## func AssociateWith

```go
func AssociateWith[K comparable, V any](slice []K, valueSelector ValueSelector[K, V]) map[K]V
```

AssociateWith returns a Map where keys are elements from the given sequence and values are produced by the valueSelector function applied to each element\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	oddEven := func(i int) string {
		if i%2 == 0 {
			return "EVEN"
		}
		return "ODD"
	}
	numbers := []int{1, 2, 3, 4}
	odsEvensMap := genfuncs.AssociateWith(numbers, oddEven)
	fmt.Println(odsEvensMap[2]) // EVEN
	fmt.Println(odsEvensMap[3]) // ODD
}
```

</p>
</details>

## func Contains

```go
func Contains[T comparable](slice []T, element T) bool
```

Contains returns true if element is found in slice\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	values := []float32{1.0, .5, 42}
	fmt.Println(genfuncs.Contains(values, .5))    // true
	fmt.Println(genfuncs.Contains(values, 3.142)) // false
}
```

</p>
</details>

## func Distinct

```go
func Distinct[T comparable](slice []T) []T
```

Distinct returns a slice containing only distinct elements from the given slice\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	values := []int{1, 2, 2, 3, 1, 3}
	fmt.Println(genfuncs.Distinct(values)) // [1 2 3]
}
```

</p>
</details>

## func Filter

```go
func Filter[T any](slice []T, predicate Predicate[T]) []T
```

Filter returns a slice containing only elements matching the given predicate\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	values := []int{1, -2, 2, -3}
	isPositive := func(i int) bool { return i > 0 }
	fmt.Println(genfuncs.Filter(values, isPositive)) // [1 2]
}
```

</p>
</details>

## func Find

```go
func Find[T any](slice []T, predicate Predicate[T]) (T, bool)
```

Find returns the first element matching the given predicate and true\, or false when no such element was found\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	values := []int{-1, -2, 2, -3}
	isPositive := func(i int) bool { return i > 0 }
	fmt.Println(genfuncs.Find(values, isPositive)) // 2 true
}
```

</p>
</details>

## func FindLast

```go
func FindLast[T any](slice []T, predicate Predicate[T]) (T, bool)
```

FindLast returns the last element matching the given predicate and true\, or false when no such element was found\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	values := []int{-1, -2, 2, 3}
	isPositive := func(i int) bool { return i > 0 }
	fmt.Println(genfuncs.FindLast(values, isPositive)) // 3 true
}
```

</p>
</details>

## func FlatMap

```go
func FlatMap[T, R any](slice []T, transform Transform[T, []R]) []R
```

FlatMap returns a slice of all elements from results of transform function being invoked on each element of original slice\, and those resultant slices concatenated\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
	"strings"
)

func main() {
	words := []string{"hello", " ", "world"}
	slicer := func(s string) []string { return strings.Split(s, "") }
	fmt.Println(genfuncs.FlatMap(words, slicer)) // [h e l l o   w o r l d]
}
```

</p>
</details>

## func Fold

```go
func Fold[T, R any](slice []T, initial R, operation Operation[T, R]) R
```

Fold accumulates a value starting with initial value and applying operation from left to right to current accumulated value and each element\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	numbers := []int{1, 2, 3, 4, 5}
	sum := func(a int, b int) int { return a + b }
	fmt.Println(genfuncs.Fold(numbers, 0, sum)) // 15
}
```

</p>
</details>

## func GroupBy

```go
func GroupBy[T any, K comparable](slice []T, keySelector KeySelector[T, K]) map[K][]T
```

GroupBy groups elements of the slice by the key returned by the given keySelector function applied to each element and returns a map where each group key is associated with a slice of corresponding elements\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	oddEven := func(i int) string {
		if i%2 == 0 {
			return "EVEN"
		}
		return "ODD"
	}
	numbers := []int{1, 2, 3, 4}
	grouped := genfuncs.GroupBy(numbers, oddEven)
	fmt.Println(grouped["ODD"]) // [1 3]
}
```

</p>
</details>

## func JoinToString

```go
func JoinToString[T any](slice []T, stringer Stringer[T], separator string, prefix string, postfix string) string
```

JoinToString creates a string from all the elements using the stringer on each\, separating them using separator\, and using the given prefix and postfix\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
	"strconv"
)

func main() {
	values := []bool{true, false, true}
	fmt.Println(genfuncs.JoinToString(values, strconv.FormatBool, ", ", "{", "}")) // {true, false, true}
}
```

</p>
</details>

## func Map

```go
func Map[T, R any](slice []T, transform Transform[T, R]) []R
```

Map returns a slice containing the results of applying the given transform function to each element in the original slice\.

<details><summary>Example</summary>
<p>

```go
package main

import (
	"fmt"
	"github.com/nwillc/genfuncs"
)

func main() {
	numbers := []int{69, 88, 65, 77, 80, 76, 69}
	toString := func(i int) string { return string(rune(i)) }
	fmt.Println(genfuncs.Map(numbers, toString)) // [E X A M P L E]
}
```

</p>
</details>

## type KeySelector

KeySelector is used for generating keys from types\, it accepts any type and returns a comparable key for it\.

```go
type KeySelector[T any, K comparable] func(T) K
```

## type Operation

Operation is used to perform operations on its arguments\, it accepts two arguments of any type and returns a result of the type of the first argument\.

```go
type Operation[T, R any] func(R, T) R
```

## type Predicate

Predicate is used evaluate a value\, it accepts any type and returns a bool\.

```go
type Predicate[T any] func(T) bool
```

## type Stringer

Stringer is used to create string representations\, it accepts any type and returns a string\.

```go
type Stringer[T any] func(T) string
```

## type Transform

Transform is used to transform values and types\, it accepts an argument of any type and returns any type\.

```go
type Transform[T, R any] func(T) R
```

## type TransformKV

TransformKV is used to generate a key and value from a type\, it accepts any type\, and returns a comparable key and any value\.

```go
type TransformKV[T any, K comparable, V any] func(T) (K, V)
```

## type ValueSelector

ValueSelector is used to select a value for a key\, given a comparable key will return a value of any type\.

```go
type ValueSelector[K comparable, T any] func(K) T
```



Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
