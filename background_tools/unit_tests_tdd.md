# Unit Tests and Test-Driven Development

## Background: What Are Unit Tests and TDD?

### Unit Tests

A **unit test** is a small, automated check that verifies a specific, isolated piece of code behaves as expected. Each test exercises one function or method — the "unit" — and asserts that given some input, the output matches what you intended.

Unit tests are valuable in research contexts for several reasons:

- They catch bugs early, before they contaminate results downstream
- They serve as executable documentation: the tests show exactly what a function is supposed to do
- They make refactoring safer — if you change the internals of a function, the tests tell you whether the behavior changed
- They support reproducibility by ensuring that each component of an analysis works correctly in isolation

### Test-Driven Development

**Test-driven development (TDD)** is a practice where you write tests *before* writing the code they test. The workflow follows a short cycle:

1. **Red** — write a test for a feature that does not yet exist; watch it fail
2. **Green** — write the minimum code needed to make the test pass
3. **Refactor** — clean up the code while keeping the tests green

This cycle is often called **Red-Green-Refactor**.

TDD forces you to think about the interface and expected behavior of a function before its implementation. This has several practical benefits:

- You end up with a test suite by construction — no need to go back and write tests later
- Edge cases and failure modes get considered up front
- Code tends to be more modular because you write it to be testable
- Debugging is faster because you know exactly when and where things broke

### TDD and AI-Assisted Coding

TDD pairs especially well with AI coding assistants. A productive workflow:

1. Write a test that specifies exactly what you want the function to do
2. Paste the test into the AI assistant and ask it to implement the function
3. Run the tests to verify the AI's output is correct
4. Iterate if tests fail

The tests serve as a precise, machine-checkable contract between you and the AI, keeping you in control of the specification while delegating the implementation.

---

## Unit Tests in Python with pytest

The standard tool for testing Python code is **pytest** — simpler to write and read than the built-in `unittest` module.

### Installation

```bash
pip install pytest
```

### A Basic Test

Suppose you have a function in `analysis.py`:

```python
def annualize_return(monthly_return):
    """Convert a monthly return to an annualized return."""
    return (1 + monthly_return) ** 12 - 1
```

A test file `test_analysis.py` might look like:

```python
from analysis import annualize_return

def test_zero_monthly_return():
    assert annualize_return(0) == 0

def test_positive_monthly_return():
    result = annualize_return(0.01)
    assert abs(result - 0.1268) < 0.0001  # ~12.68% annualized

def test_negative_monthly_return():
    result = annualize_return(-0.01)
    assert result < 0
```

### Running Tests

```bash
# Run all tests in the current directory
pytest

# Run a specific file
pytest test_analysis.py

# Run a specific test function
pytest test_analysis.py::test_zero_monthly_return

# Show verbose output
pytest -v
```

### Testing Floating-Point Results

Economic calculations often produce floating-point numbers. Use `pytest.approx` rather than exact equality:

```python
import pytest
from analysis import annualize_return

def test_annualized_return():
    assert annualize_return(0.01) == pytest.approx(0.12683, rel=1e-4)
```

### Testing for Expected Errors

Sometimes the right behavior is to raise an exception. Test that too:

```python
import pytest
from analysis import compute_gini

def test_gini_rejects_negative_values():
    with pytest.raises(ValueError):
        compute_gini([-100, 200, 300])
```

### TDD Example: Calculating a Poverty Rate

**Step 1 — Write the tests first:**

```python
# tests/test_poverty.py
import pytest
from poverty import poverty_rate

def test_all_above_threshold():
    incomes = [30000, 45000, 60000]
    assert poverty_rate(incomes, threshold=20000) == 0.0

def test_all_below_threshold():
    incomes = [5000, 8000, 12000]
    assert poverty_rate(incomes, threshold=20000) == 1.0

def test_half_below_threshold():
    incomes = [10000, 10000, 30000, 30000]
    assert poverty_rate(incomes, threshold=20000) == 0.5

def test_empty_list_raises():
    with pytest.raises(ValueError):
        poverty_rate([], threshold=20000)
```

Run `pytest` — all four tests fail because `poverty.py` does not exist yet. That is the **Red** step.

**Step 2 — Write the minimum code to pass:**

```python
# poverty.py
def poverty_rate(incomes, threshold):
    if len(incomes) == 0:
        raise ValueError("incomes list cannot be empty")
    below = sum(1 for income in incomes if income < threshold)
    return below / len(incomes)
```

Run `pytest` — all tests pass. That is the **Green** step.

**Step 3 — Refactor if needed.** The implementation is already simple, so nothing to change.

### Fixtures and Parametrize

Use **fixtures** to share setup data across tests without repetition:

```python
import pytest
import pandas as pd

@pytest.fixture
def sample_dataframe():
    return pd.DataFrame({
        "income": [15000, 25000, 40000, 80000],
        "employed": [True, True, False, True]
    })

def test_mean_income(sample_dataframe):
    assert sample_dataframe["income"].mean() == 40000

def test_employment_rate(sample_dataframe):
    assert sample_dataframe["employed"].mean() == pytest.approx(0.75)
```

Use `@pytest.mark.parametrize` to run the same test across multiple inputs:

```python
@pytest.mark.parametrize("monthly, expected", [
    (0.00, 0.0000),
    (0.01, 0.1268),
    (0.05, 0.7959),
])
def test_annualize_return_cases(monthly, expected):
    assert annualize_return(monthly) == pytest.approx(expected, rel=1e-3)
```

### Organizing Tests

A common project layout keeps tests in a `tests/` directory:

```
project/
├── analysis.py
├── data_cleaning.py
└── tests/
    ├── test_analysis.py
    └── test_data_cleaning.py
```

pytest discovers test files automatically when they are named `test_*.py` or `*_test.py` and test functions are prefixed with `test_`.

### Integration with GitHub Actions

Tests run automatically on every push or pull request when combined with a CI workflow. A minimal `.github/workflows/tests.yml`:

```yaml
name: Run Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: pip install pytest
      - run: pytest
```

---

## Unit Tests in R with testthat

The standard testing framework for R is **testthat**, developed by Hadley Wickham and part of the tidyverse ecosystem.

### Installation

```r
install.packages("testthat")
```

If you are working inside an R package (the most common context for testthat), `usethis` can set up the testing infrastructure for you:

```r
install.packages("usethis")
usethis::use_testthat()
```

This creates a `tests/testthat/` directory and a `tests/testthat.R` runner file.

### A Basic Test

Suppose you have a function in `R/analysis.R`:

```r
annualize_return <- function(monthly_return) {
  (1 + monthly_return)^12 - 1
}
```

A test file `tests/testthat/test-analysis.R` might look like:

```r
library(testthat)

test_that("zero monthly return gives zero annual return", {
  expect_equal(annualize_return(0), 0)
})

test_that("positive monthly return gives positive annual return", {
  expect_equal(annualize_return(0.01), 0.12683, tolerance = 1e-4)
})

test_that("negative monthly return gives negative annual return", {
  expect_lt(annualize_return(-0.01), 0)
})
```

### Running Tests

```r
# Run all tests in tests/testthat/
testthat::test_dir("tests/testthat")

# If inside a package, use devtools:
devtools::test()

# Run a single test file
testthat::test_file("tests/testthat/test-analysis.R")
```

### Common Expectation Functions

| Function | Checks |
|---|---|
| `expect_equal(x, y)` | `x` equals `y` (with optional `tolerance`) |
| `expect_identical(x, y)` | `x` is identical to `y` (strict, no tolerance) |
| `expect_true(x)` | `x` is `TRUE` |
| `expect_false(x)` | `x` is `FALSE` |
| `expect_lt(x, y)` | `x` is less than `y` |
| `expect_gt(x, y)` | `x` is greater than `y` |
| `expect_error(expr)` | `expr` throws an error |
| `expect_warning(expr)` | `expr` produces a warning |
| `expect_message(expr)` | `expr` produces a message |
| `expect_length(x, n)` | `x` has length `n` |
| `expect_named(x, names)` | `x` has the given names |

### TDD Example: A Poverty Rate Function in R

**Step 1 — Write the tests first:**

```r
# tests/testthat/test-poverty.R
library(testthat)

test_that("all incomes above threshold gives poverty rate of 0", {
  expect_equal(poverty_rate(c(30000, 45000, 60000), threshold = 20000), 0)
})

test_that("all incomes below threshold gives poverty rate of 1", {
  expect_equal(poverty_rate(c(5000, 8000, 12000), threshold = 20000), 1)
})

test_that("half below threshold gives poverty rate of 0.5", {
  expect_equal(poverty_rate(c(10000, 10000, 30000, 30000), threshold = 20000), 0.5)
})

test_that("empty vector raises an error", {
  expect_error(poverty_rate(numeric(0), threshold = 20000))
})
```

Run the tests — they all fail. **Red.**

**Step 2 — Implement the function:**

```r
# R/poverty.R
poverty_rate <- function(incomes, threshold) {
  if (length(incomes) == 0) stop("incomes vector cannot be empty")
  mean(incomes < threshold)
}
```

Run the tests — they all pass. **Green.**

### Fixtures with `setup` and Helper Files

For shared data or objects, place helper functions in `tests/testthat/helper.R` (testthat loads this automatically):

```r
# tests/testthat/helper.R
make_sample_data <- function() {
  data.frame(
    income   = c(15000, 25000, 40000, 80000),
    employed = c(TRUE, TRUE, FALSE, TRUE)
  )
}
```

Then use it in any test file:

```r
test_that("mean income is computed correctly", {
  df <- make_sample_data()
  expect_equal(mean(df$income), 40000)
})
```

### Skipping Tests

You can skip a test conditionally, for example when a required package is not installed or when running on CI:

```r
test_that("function works with optional dependency", {
  skip_if_not_installed("data.table")
  # ... test body
})
```

### Integration with GitHub Actions

```yaml
name: Run R Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: r-lib/actions/setup-r@v2
      - run: Rscript -e "install.packages(c('testthat', 'devtools'))"
      - run: Rscript -e "devtools::test()"
```

---

## Unit Tests in Stata

Stata's testing ecosystem is considerably thinner than Python's or R's, but useful options exist. There is no single widely adopted framework equivalent to pytest or testthat — the practical approach combines built-in commands with a conventional script structure.

### The Foundation: `assert`

`assert` is Stata's built-in assertion command. It evaluates a condition across all observations (or as a scalar expression) and throws an error if the condition is false, stopping execution immediately:

```stata
* Check that no negative values exist after data cleaning
assert income >= 0

* Check a scalar result
assert e(N) == 1000
assert abs(_b[x1] - 2) < 0.5
```

This is the building block for all Stata testing.

### Certification Scripts with `cscript`

`cscript` is an undocumented but widely used Stata command — StataCorp uses it internally to test official commands, and professional ado-file developers use it before releasing packages. A certification script is a `.do` file that exercises your program against data with known properties and uses `assert` to verify results.

The core idea: generate synthetic data where you know the true answer, run your estimator, and assert the output matches expectations.

```stata
* test_myreg.do — certification script for myreg.ado

cscript "myreg: basic OLS recovery"

* Generate data with known DGP: y = 2*x + noise
clear
set seed 12345
set obs 500
gen x = rnormal()
gen y = 2 * x + rnormal()

* Run the estimator under test
myreg y x

* Assert expected behavior
assert abs(_b[x] - 2) < 0.15          // coefficient near true value of 2
assert abs(_b[_cons]) < 0.15          // intercept near true value of 0
assert e(df_r) == 498                  // correct degrees of freedom
assert !missing(e(r2))                 // R-squared was computed
assert e(r2) > 0 & e(r2) < 1         // R-squared is in [0,1]
```

Run with `do test_myreg.do`. A failed `assert` stops the script and returns a non-zero exit code, which CI systems can detect.

### Testing That Errors Are Raised

Use `capture` to test that your program correctly rejects bad input:

```stata
* myreg should error if fewer than 2 observations
clear
set obs 1
gen y = 5
gen x = 3

capture myreg y x
assert _rc != 0    // confirm a non-zero return code (i.e., an error occurred)
```

### Organizing Test Scripts

A simple project layout:

```
project/
├── myreg.ado
├── myreg2.ado
└── tests/
    ├── test_myreg.do
    └── test_myreg2.do
```

Running all tests:

```bash
stata -b do tests/test_myreg.do
stata -b do tests/test_myreg2.do
```

### For Mata Code: `TESTCASE`

If your estimator uses Mata for numerical routines, the community-written `TESTCASE` package (available on SSC) provides a more structured, xUnit-style framework with explicit pass/fail reporting:

```stata
ssc install testcase
```

This is the closest Stata equivalent to pytest or testthat, but it only applies to Mata code, not ado-files.

### Integration with GitHub Actions

```yaml
name: Run Stata Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run certification scripts
        run: |
          stata -b do tests/test_myreg.do
          grep -c "r(9)" tests/test_myreg.log && exit 1 || exit 0
```

```{note}
Running Stata in CI requires a license. Some teams use a Stata license server or the `stata-ci` Docker image. An alternative is to test the core numerical routines in a language with better CI support (Python or R) and reserve Stata tests for local runs.
```

### Honest Assessment

| Tool | Best for | Maturity |
|------|----------|----------|
| `assert` | Quick sanity checks in any `.do` file | Built-in |
| `cscript` | Testing ado-files before release | Undocumented but widely used |
| `TESTCASE` | Testing Mata functions | Community package (SSC) |

The most practical approach for someone writing econometrics estimators in Stata: write a dedicated `test_yourcommand.do` that generates synthetic data with known properties (so you know the true answer), runs your program, and uses `assert` to verify the results. This covers the most important case — checking that your estimator recovers the right answer — even without a formal testing framework.

---

## Further Resources

- [pytest Documentation](https://docs.pytest.org/) — official reference for pytest features and configuration
- [Real Python: Getting Started with Testing](https://realpython.com/python-testing/) — a practical introduction to testing in Python
- [testthat Documentation](https://testthat.r-lib.org/) — official reference for testthat
- [R Packages: Testing (Hadley Wickham)](https://r-pkgs.org/testing-basics.html) — the definitive guide to testing in R packages
- [Test-Driven Development with Python](https://www.obeythetestinggoat.com/) — a free book using TDD to build a web application step by step
- [Statistical Software Certification (Gould, 2001)](https://www.stata-journal.com/article.html?article=pr0001) — the Stata Journal article that introduced `cscript` and the certification script approach
- [TESTCASE on SSC](https://ideas.repec.org/c/boc/bocode/s457826.html) — Mata-based unit testing framework for Stata
