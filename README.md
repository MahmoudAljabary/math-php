Math PHP
=====================

A modern math library for PHP.

Math PHP is a self-contained mathematics library in pure PHP with no external dependencies. It is actively under development and should be considered a work in progress.

Features
--------
 * Probability
     * Combinatorics
     * Distributions
 * Statistics
     * Averages
     * Descriptive
     * Distributions
     * Random Variables

Setup
-----

 Add the library to your `composer.json` file in your project:

```javascript
{
  "require": {
      "markrogoyski/math-php": "0.0.*"
  }
}
```

Use [composer](http://getcomposer.org) to install the library:

```bash
$ php composer.phar install
```

Composer will install Math PHP inside your vendor folder. Then you can add the following to your
.php files to use the library with Autoloading.

```php
require_once( __DIR__ . '/vendor/autoload.php' );
```

### Minimum Requirements
 * PHP 7

Usage
-----

### Probability - Combinatorics
```php
use Math\Probability\Combinatorics;

// Factorial and permutations
$factorial    = Combinatorics::factorial(5);    // Same as permutations
$permutations = Combinatorics::permutations(5); // Same as factorial

// Permutations n choose r
$n = 10;
$r = 4;
$permutations = Combinatorics::permutationsChooseR( $n, $r );

// Combinations
$combinations = Combinatorics::combinations( $n, $r );
$combinations = Combinatorics::combinationsWithRepetition( $n, $r );

// Multinomial Theorem
$n         = 10;
$groups    = [ 5, 2, 3 ];
$divisions = Combinatorics::multinomialTheorem( $n, $groups );
```

### Probability - Distributions
```php
use Math\Probability\Distribution;

// Binomial distribution
$n = 2;   // number of events
$r = 1;   // number of successful events
$P = 0.5; // probability of success
$binomial = Distribution::binomial( $n, $r, $P );

// Negative binomial distribution (Pascal)
$x = 2;   // number of trials required to produce r successes
$r = 1;   // number of successful events
$P = 0.5; // probability of success on an individual trial
$negative_binomial = Distribution::negativeBinomial( $x, $r, $P );  // Same as pascal
$pascal            = Distribution::pascal( $x, $r, $P );            // Same as negative binomial

// Poisson distribution
$k = 3; // events in the interval
$λ = 2; // average number of successful events per interval
$poisson            = Distribution::poisson( $k, $λ );
$cumulative_poisson = Distribution::cumulativePoisson( $k, $λ );
```

### Statistics - Averages
```php
use Math\Statistics\Average;

$numbers = [ 13, 18, 13, 14, 13, 16, 14, 21, 13 ];

// Mean, median, mode
$mean   = Average::mean($numbers);
$median = Average::median($numbers);
$mode   = Average::mode($numbers); // Returns an array -- may be multimodal

// Averages report
// Returns array with keys: mean, median and mode
$averages = Average::getAverages($numbers);
```

### Statistics - Descriptive
```php
use Math\Statistics\Descriptive

$numbers = [ 13, 18, 13, 14, 13, 16, 14, 21, 13 ];

// Range and midrange
$range    = Descriptive::range($numbers);
$midrange = Descriptive::midrange($numbers);

// Variance (population and sample)
$σ² = Descriptive::populationVariance($numbers);
$S² = Descriptive::sampleVariance($numbers);

// Standard deviation
$σ = Descriptive::standardDeviation($numbers); // Has optional parameter to set population or sample variance

// MAD - mean/median absolute deviations
$mean_mad   = Descriptive::meanAbsoluteDeviation($numbers);
$median_mad = Descriptive::medianAbsoluteDeviation($numbers);

// Quartiles
$quartiles = Descriptive::quartiles($numbers);
// [ 0% => 13, 25% => 13, 50% => 14, 75% => 17, 100% => 21, IQR => 4 ]

// IQR - Interquartile range
$IQR = Descriptive::interquartileRange($numbers); // Same as IQR
$IQR = Descriptive::IQR($numbers);                // Same as interquartileRange

// Percentiles
$twentieth_percentile    = Descriptive::percentile( $numbers, 20 );
$ninety_fifth_percentile = Descriptive::percentile( $numbers, 95 );

// Descriptive stats report
// Returns array with keys: mean, median, mode, range, midrange, variance, standard deviation, mean_mad, median_mad, quartiles
$stats = Descriptive::getStats($numbers); // Has optional parameter to set population or sample variance
```
### Statistics - Distributions
```php
use Math\Statistics\Distribution

$grades = [ 'A', 'A', 'B', 'B', 'B', 'B', 'C', 'C', 'D', 'F' ];

// Frequency distributions (frequency and relative frequency)
$frequencies          = Distribution::frequency($grades);         // [ A => 2,   B => 4,   C => 2,   D => 1,   F => 1   ]
$relative_frequencies = Distribution::relativeFrequency($grades); // [ A => 0.2, B => 0.4, C => 0.2, D => 0.1, F => 0.1 ]

// Cumulative frequency distributions (cumulative and cumulative relative)
$cumulative_frequencies          = Distribution::cumulativeFrequency($grades);         // [ A => 2,   B => 6,   C => 8,   D => 9,   F => 10  ]
$cumulative_relative_frequencies = Distribution::cumulativeRelativeFrequency($grades); // [ A => 0.2, B => 0.6, C => 0.8, D => 0.9, F => 1   ]

// Stem and leaf plot
// Return value is array where keys are the stems, values are the leaves
$values             = [ 44, 46, 47, 49, 63, 64, 66, 68, 68, 72, 72, 75, 76, 81, 84, 88, 106 ];
$stem_and_leaf_plot = Distribution::stemAndLeafPlot($values);
// [ 4 => [4, 6, 7, 9], 5 => [], 6 => [3, 4, 6, 8, 8], 7 => [2, 2, 5, 6], 8 => [1, 4, 8], 9 => [], 10 => [6] ]

// Optional second parameter will print stem and leaf plot to STDOUT
Distribution::stemAndLeafPlot( $values, Distribution::PRINT );
/*
 4 | 4 6 7 9
 5 |
 6 | 3 4 6 8 8
 7 | 2 2 5 6
 8 | 1 4 8
 9 |
10 | 6
*/
```

### Statistics - Random Variables
```php
use Math\Statistics\RandomVariable

$X = [ 1, 2, 3, 4 ];
$Y = [ 2, 3, 4, 5 ];

// Covariance (population and sample)
$σxy = RandomVariable::populationCovariance( $X, $Y );
$Sxy = RandomVariable::sampleCovariance( $X, $Y );

// Correlation coefficient (population and sample)
$ρxy = RandomVariable::populationCorrelationCoefficient( $X, $Y );
$rxy = RandomVariable::sampleCorrelationCoefficient( $X, $Y );

// Central moment (nth moment)
$second_central_moment = RandomVariable::centralMoment( $X, 2 );
$third_central_moment  = RandomVariable::centralMoment( $X, 3 );

// Skewness (population and sample)
$skewness = RandomVariable::populationSkewness($X);  // Similar to Excel's SKEW.P
$skewness = RandomVariable::sampleSkewness($X);      // Similar to Excel's SKEW
// Alernative method of calculating skewness
$skewness = RandomVariable::skewness($X);
```

Unit Tests
----------

```bash
$ cd tests
$ phpunit .
```