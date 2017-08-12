[![Build Status](https://travis-ci.org/lambdaisland/uniontypes.svg?branch=master)](https://travis-ci.org/lambdaisland/uniontypes)
[![Clojars Project](https://img.shields.io/clojars/v/lambdaisland/uniontypes.svg)](https://clojars.org/lambdaisland/uniontypes)

# lambdaisland/uniontypes

Union Types (Algebraic Data Types) for Clojure and ClojureScript, based on clojure.spec.

Provides a `case-of` macro, which does case matching based on which branch in an
`or` spec a value conforms to.

`case-of` checks at compile time that all cases are handled, and if not throws an exception.

## Installation

To install, add the following dependency to your project or build file:

``` clojure
[lambdaisland/uniontypes "0.3.0"]
```

## Usage

``` clojure
(require '[lambdaisland.uniontypes :refer [case-of]])

(s/def ::availability (s/or :sold-out  #{:sold-out}
                            :in-stock  pos-int?
                            :reordered (s/tuple pos-int? pos-int?)
                            :announced string?))

(defn display-status [availability]
  (case-of ::availability availability
    :sold-out _
    "Sold out."

    :in-stock amount
    (str "In stock: " amount " items left.")

    :reordered [min max]
    (str "Available again in " min " to " max "days" )

    :announced date
    (str "Available on " date ".")

    :spec/invalid msg
    (str "Not a valid availability: " msg)))
```

## License

Copyright © 2016-2017 Arne Brasseur

Distributed under the Mozille Public License 2.0
