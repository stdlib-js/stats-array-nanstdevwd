<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# nanstdevwd

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Calculate the [standard deviation][standard-deviation] of an array ignoring `NaN` values and using Welford's algorithm.

<section class="intro">

The population [standard deviation][standard-deviation] of a finite size population of size `N` is given by

<!-- <equation class="equation" label="eq:population_standard_deviation" align="center" raw="\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}" alt="Equation for the population standard deviation."> -->

```math
\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}
```

<!-- <div class="equation" align="center" data-raw-text="\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}" data-equation="eq:population_standard_deviation">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@6ed5b63b6ce33d2915ce2862c86d55ae33cdb609/lib/node_modules/@stdlib/stats/strided/nanstdevwd/docs/img/equation_population_standard_deviation.svg" alt="Equation for the population standard deviation.">
    <br>
</div> -->

<!-- </equation> -->

where the population mean is given by

<!-- <equation class="equation" label="eq:population_mean" align="center" raw="\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i" alt="Equation for the population mean."> -->

```math
\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i
```

<!-- <div class="equation" align="center" data-raw-text="\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i" data-equation="eq:population_mean">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@6ed5b63b6ce33d2915ce2862c86d55ae33cdb609/lib/node_modules/@stdlib/stats/strided/nanstdevwd/docs/img/equation_population_mean.svg" alt="Equation for the population mean.">
    <br>
</div> -->

<!-- </equation> -->

Often in the analysis of data, the true population [standard deviation][standard-deviation] is not known _a priori_ and must be estimated from a sample drawn from the population distribution. If one attempts to use the formula for the population [standard deviation][standard-deviation], the result is biased and yields an **uncorrected sample standard deviation**. To compute a **corrected sample standard deviation** for a sample of size `n`,

<!-- <equation class="equation" label="eq:corrected_sample_standard_deviation" align="center" raw="s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}" alt="Equation for computing a corrected sample standard deviation."> -->

```math
s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}
```

<!-- <div class="equation" align="center" data-raw-text="s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}" data-equation="eq:corrected_sample_standard_deviation">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@6ed5b63b6ce33d2915ce2862c86d55ae33cdb609/lib/node_modules/@stdlib/stats/strided/nanstdevwd/docs/img/equation_corrected_sample_standard_deviation.svg" alt="Equation for computing a corrected sample standard deviation.">
    <br>
</div> -->

<!-- </equation> -->

where the sample mean is given by

<!-- <equation class="equation" label="eq:sample_mean" align="center" raw="\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i" alt="Equation for the sample mean."> -->

```math
\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i
```

<!-- <div class="equation" align="center" data-raw-text="\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i" data-equation="eq:sample_mean">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@6ed5b63b6ce33d2915ce2862c86d55ae33cdb609/lib/node_modules/@stdlib/stats/strided/nanstdevwd/docs/img/equation_sample_mean.svg" alt="Equation for the sample mean.">
    <br>
</div> -->

<!-- </equation> -->

The use of the term `n-1` is commonly referred to as Bessel's correction. Note, however, that applying Bessel's correction can increase the mean squared error between the sample standard deviation and population standard deviation. Depending on the characteristics of the population distribution, other correction factors (e.g., `n-1.5`, `n+1`, etc) can yield better estimators.

</section>

<!-- /.intro -->

<section class="installation">

## Installation

```bash
npm install @stdlib/stats-array-nanstdevwd
```

Alternatively,

-   To load the package in a website via a `script` tag without installation and bundlers, use the [ES Module][es-module] available on the [`esm`][esm-url] branch (see [README][esm-readme]).
-   If you are using Deno, visit the [`deno`][deno-url] branch (see [README][deno-readme] for usage intructions).
-   For use in Observable, or in browser/node environments, use the [Universal Module Definition (UMD)][umd] build available on the [`umd`][umd-url] branch (see [README][umd-readme]).

The [branches.md][branches-url] file summarizes the available branches and displays a diagram illustrating their relationships.

To view installation and usage instructions specific to each branch build, be sure to explicitly navigate to the respective README files on each branch, as linked to above.

</section>

<section class="usage">

## Usage

```javascript
var nanstdevwd = require( '@stdlib/stats-array-nanstdevwd' );
```

#### nanstdevwd( x\[, correction] )

Computes the [standard deviation][standard-deviation] of an array ignoring `NaN` values and using Welford's algorithm.

```javascript
var x = [ 1.0, -2.0, NaN, 2.0 ];

var v = nanstdevwd( x );
// returns ~2.0817
```

The function has the following parameters:

-   **x**: input array.
-   **correction**: degrees of freedom adjustment. Setting this parameter to a value other than `0` has the effect of adjusting the divisor during the calculation of the [standard deviation][standard-deviation] according to `N-c` where `N` corresponds to the number of non-`NaN` array elements and `c` corresponds to the provided degrees of freedom adjustment. When computing the [standard deviation][standard-deviation] of a population, setting this parameter to `0` is the standard choice (i.e., the provided array contains data constituting an entire population). When computing the unbiased sample [standard deviation][standard-deviation], setting this parameter to `1` is the standard choice (i.e., the provided array contains data sampled from a larger population; this is commonly referred to as Bessel's correction). Default: `1.0`.

By default, the function computes the sample [standard deviation][standard-deviation]. To adjust the degrees of freedom when computing the [standard deviation][standard-deviation], provide a `correction` argument.

```javascript
var x = [ 1.0, -2.0, NaN, 2.0 ];

var v = nanstdevwd( x, 0.0 );
// returns ~1.6997
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If provided an empty array, the function returns `NaN`.
-   If `N - c` is less than or equal to `0` (where `c` corresponds to the provided degrees of freedom adjustment and `N` corresponds to the number of non-`NaN` array elements), the function returns `NaN`.
-   The function supports array-like objects having getter and setter accessors for array element access (e.g., [`@stdlib/array-base/accessor`][@stdlib/array/base/accessor]).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
var uniform = require( '@stdlib/random-base-uniform' );
var filledarrayBy = require( '@stdlib/array-filled-by' );
var bernoulli = require( '@stdlib/random-base-bernoulli' );
var nanstdevwd = require( '@stdlib/stats-array-nanstdevwd' );

function rand() {
    if ( bernoulli( 0.8 ) < 1 ) {
        return NaN;
    }
    return uniform( -50.0, 50.0 );
}

var x = filledarrayBy( 10, 'generic', rand );
console.log( x );

var v = nanstdevwd( x );
console.log( v );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2025. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-array-nanstdevwd.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-array-nanstdevwd

[test-image]: https://github.com/stdlib-js/stats-array-nanstdevwd/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/stats-array-nanstdevwd/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-array-nanstdevwd/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-array-nanstdevwd?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-array-nanstdevwd.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-array-nanstdevwd/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-array-nanstdevwd/tree/deno
[deno-readme]: https://github.com/stdlib-js/stats-array-nanstdevwd/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/stats-array-nanstdevwd/tree/umd
[umd-readme]: https://github.com/stdlib-js/stats-array-nanstdevwd/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/stats-array-nanstdevwd/tree/esm
[esm-readme]: https://github.com/stdlib-js/stats-array-nanstdevwd/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/stats-array-nanstdevwd/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-array-nanstdevwd/main/LICENSE

[standard-deviation]: https://en.wikipedia.org/wiki/Standard_deviation

[@stdlib/array/base/accessor]: https://github.com/stdlib-js/array-base-accessor

</section>

<!-- /.links -->
