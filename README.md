<!-- Code generated by gomarkdoc. DO NOT EDIT -->

# genfuncs

```go
import "github.com/nwillc/genfuncs"
```

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
- [func FlatMap[T, R any](slice []T, transform Transform[T, R]) []R](<#func-flatmap>)
- [func Fold[T, R any](slice []T, initial R, operation Operation[T, R]) R](<#func-fold>)
- [func JoinToString[T any](slice []T, stringer Stringer[T], separator string, prefix string, postfix string) string](<#func-jointostring>)
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

## func Any

```go
func Any[T any](slice []T, predicate Predicate[T]) bool
```

Any returns true if any element of the slice matches the predicate\.

## func Associate

```go
func Associate[T any, K comparable, V any](slice []T, transform TransformKV[T, K, V]) map[K]V
```

Associate returns a map containing key/values provided by transform function applied to elements of the slice\.

## func AssociateWith

```go
func AssociateWith[K comparable, V any](slice []K, valueSelector ValueSelector[K, V]) map[K]V
```

AssociateWith returns a Map where keys are elements from the given sequence and values are produced by the valueSelector function applied to each element\.

## func Contains

```go
func Contains[T comparable](slice []T, element T) bool
```

Contains returns true if element is found in slice\.

## func Distinct

```go
func Distinct[T comparable](slice []T) []T
```

Distinct returns a slice containing only distinct elements from the given slice\.

## func Filter

```go
func Filter[T any](slice []T, predicate Predicate[T]) []T
```

Filter returns a slice containing only elements matching the given predicate\.

## func Find

```go
func Find[T any](slice []T, predicate Predicate[T]) (T, bool)
```

Find returns the first element matching the given predicate and true\, or false when no such element was found\.

## func FindLast

```go
func FindLast[T any](slice []T, predicate Predicate[T]) (T, bool)
```

FindLast returns the last element matching the given predicate and true\, or false when no such element was found\.

## func FlatMap

```go
func FlatMap[T, R any](slice []T, transform Transform[T, R]) []R
```

FlatMap returns a slice of all elements from results of transform function being invoked on each element of original slice\.

## func Fold

```go
func Fold[T, R any](slice []T, initial R, operation Operation[T, R]) R
```

Fold accumulates a value starting with initial value and applying operation from left to right to current accumulated value and each element\.

## func JoinToString

```go
func JoinToString[T any](slice []T, stringer Stringer[T], separator string, prefix string, postfix string) string
```

JoinToString creates a string from all the elements using the stringer on each\, separating them using separator\, and using the given prefix and postfix\.

## type KeySelector

```go
type KeySelector[T any, K comparable] func(T) K
```

## type Operation

```go
type Operation[T, R any] func(R, T) R
```

## type Predicate

```go
type Predicate[T any] func(T) bool
```

## type Stringer

```go
type Stringer[T any] func(T) string
```

## type Transform

```go
type Transform[T, R any] func(T) R
```

## type TransformKV

```go
type TransformKV[T any, K comparable, V any] func(T) (K, V)
```

## type ValueSelector

```go
type ValueSelector[K comparable, T any] func(K) T
```



Generated by [gomarkdoc](<https://github.com/princjef/gomarkdoc>)
