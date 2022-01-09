# Personal notes

## 1. Equalities
- Check that multiple values are equal: `(= A B C)`
- We use `=` for strict equality `(= false (= 1 1.0))`
- We use `==` for loose equality `(= true (== 1 1.0))`
- Boolean negation is achived with `not`. It can be used before `=` or before a value: `(= true (not false))` or `(not= true false)`
- Strings (Java strings) are wrapped by double quotes (`"`): `"Hello"`
- Keywords are similar to Elixir atoms, they are cached and reused to save memory: `:hello`
- Symbols are identifiers used to refer to something else (for example, they are used to name functions and variables). They are led by single quotes: `'hello` 

## 2. Strings
- `str` is used to cast values to string and to concatenate strings: `(str 1)`, `(str "hello" 'world)`.
- Characters are lead by `\`, for example: `\C`. But chars are not strings: `(not= \c "c")`.

## 3. Lists
- Both `'(1 2 3)` and `(list 1 2 3)` are lists of 3 integers.
- Lists are sequences of chained elements only the access to the first element is cheap.
- **[?]: What's `dracula` in the following list? A Symbol? `'(dracula)`**
- **[?]: What's the difference between `first` and `peek`? And between `rest` and `pop`? [SOLUTION] They are the same when applied to lists.**