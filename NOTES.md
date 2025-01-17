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
- What's the difference between `first` and `peek`? And between `rest` and `pop`? [SOLUTION] They are the same when applied to lists.
- : What's `dracula` in the following list? A Symbol? `'(dracula)`. **[SOLUTION] It's a symbol.** 

## 4. Vectors
- Vectors are arrays and can be defined with `[]`: `[1 2 3]`.
- `vector element1 element2...` to build a new vector.
- `vec '(list)` to create a vector from a list.
- `conj` works differently in vectors and lists. In lists, the element is prepended. In vector, it's appended.
- A list and a vector are equal if they have the same values (in the same order).

## 5. Sets
- Sets are mathematical sets (a collection without duplicates nor order) and they can be defined with `#{}`: `#{1 3 2}`.

## 6. Maps
- Hash maps are defined with `{}`: `{:key "value" "key2" 234 "value"}` (their keys can be anything, not only keywords).
- Both keys and maps can be used as functions to retrieve elements: `(= 23 ({:a 23} :a) (:a {:a 23}))`.
- `assoc` to add new elements to a map (actually, it creates a new map, everything is immutable in Clojure): `assoc {:a 1} :b 2`.
- `merge` to merge two maps: `merge {:a 1} {:b 2}`.
- `vals` to get a list with the map values and `keys` for the keys.

## 7. Functions
- `defn function-name [PARAMETER1 PARAMETER2] (IMPLEMENTATION)`.
- Or inline (anonymous): `fn [PARAMETER1 PARAMETER2] (IMPLMENTATION)`.
- Shorter inline syntax to sum parameter 1 and 2: `#(+ &1 &2)`.

## 8. Conditions
- `if (CONDITION) THEN`
- `if (CONDITION) THEN ELSE`
- `case (EXPRESSION) VALUE1 THEN1 VALUE2 THEN2 DEFAULT`
- `cond (CONDITION1) THEN1 (CONDITION2) THEN2 :else DEFAULT`

## 10. Runtime polymorphism (function pattern matching)

By number of parameters:
````
(defn hello
  ([] "Hello World!")
  ([a] (str "Hello, you silly " a "."))
  ([a & more] (str 
    "Hello to this group: "
    (apply str (interpose ", " (cons a more))) "!")))
````

By parameter value:
````
(defmulti diet (fn [x] (:eater x)))
(defmethod diet :herbivore [{:keys [name]}] (str name " eats veggies."))
(defmethod diet :carnivore [_a] "Simba eats animals.")
(defmethod diet :default [_a] "I don't know what Rich Hickey eats.")
````

## 12. Sequence comprehensions
`(= 
    '(0 1 4)
     (for [x (range 30) :when (< x 3)] (* x x)))`

## 13. Creating functions
- `let [CONTEXT] EXPRESSION`: `let [x 5] (str "The value of x is " x)`
- `comp` for composition. For example, get the square of a value and then add 2 `comp inc inc square`.

## 14. Recursion
Regular:
````
(defn factorial [n]
  (if (= n 1) 
    1
    (* n (factorial (dec n)))))  
````
Optimized to avoid stack overflow:
````
(defn factorial-optimized [n]
  (loop [n n acc 1]
    (if (= n 1) 
        acc
        (recur (dec n) (* acc n)))))
````

## 15. Destructuring
- For vectors: `fn [[first second & rest]] ...`
- For maps: `fn [{variable :map-key}] ...`
- For maps, shorter: `fn [{:keys [map-key1 map-key2]}] ...`

## 17. Atoms
For mutable state.
- Definition: `(def this-can-change (atom "foo")`
- Getting its value: `@this-can-change`
- Update: `(swap! this-can-change FUNCTION)` => `(swap! this-can-change inc)`
- Update only if the atom has certain value: `(compare-and-set! this-can-change CURRENT_VALUE NEW VALUE)`=> Update the atom with NEW VALUE if and only if the current value is CURRENT_VALUE.

## 18. Quotes
Use `quote` or `'` before an expression to avoid evaluating it and consider it data instead. 
- For example: `(= '(1 2 4) (quote (1 2 4)) [1 2 4])`
- The syntax quote **\`** does the same but lets you "unquote" an expression with `~`:
**(= [1 3] \`(1 ~(+ 1 2)))**

## 20. Java interoperation
- Use a leading dot for methods: `(= "FOO" (.toUpperCase "foo"))`.
- Use a dot at the end to instantiate a class: `(= 23.0 (Float. "23"))`.
- Use `Class/method` to call static methods: `(= 2.0 (Math/ceil 1.9))`

## 22. Group by
To generate a map of elements from a collection, grouped by the specified criteria: `(group-by FUNCTION COLLECTION)`
- `(= {5 ["hello" "world"] 3 ["foo" "bar"]} (group-by count ["hello" "world" "foo" "bar"]))`

You can also use a `key` to group-by over a collection of maps:
- `(group-by :id [{:id 1 :name "Bob"} {:id 2 :name "Jennifer"} {:id 1 :last-name "Smith"} ])`

## 24. Macros
Functions created at compile time: `(defmacro NAME [PARAMS] LOGIC)`.

This is interesting: `(macroexpand '(MACRONAME (9 2 1)))`. You can run a macro from data using `macroexpand` and a collection with the name of the macro as the first element.

## 25. Threading macros (pipe operator)
- `->` to pass the output of the previous function as the first parameter of the next one.
- `->>` to pass the output of the previous function as the last parameter of the next one.

## 27. Multimethods
Similar to Elixir pattern matching, although not as easy to use IMHO (probably I don't understand how to use it properly yet).

Given a fucntion within a pipe, that should do something different depeding on the result of the previous one (:ok, error pattern).

**Elixir**
```
def whatever({:ok, data}), do: do_whatever(data);
def whatever({:error, _}), do: log("something went wrong");
```

**Clojure**
```
(defmulti whatever (fn [{result _data}] result))
(defmethod whatever :ok [_ data] do_whatever(data))
(defmethod whatever :error [_ _] log_error("something went wrong"))
```