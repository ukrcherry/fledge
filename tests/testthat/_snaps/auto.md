# guess_next_impl() works

    Code
      guess_next_impl("1.2.3.9007")
    Output
      [1] "patch"
    Code
      guess_next_impl("1.2.99.9008")
    Output
      [1] "minor"
    Code
      guess_next_impl("1.99.99.9009")
    Output
      [1] "major"

