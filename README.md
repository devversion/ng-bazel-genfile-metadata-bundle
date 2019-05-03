# ng-bazel-genfile-metadata-bundle

Run `bazel build //src` and inspect the `bazel-bin` output. It can be observed that the flat module metadata bundle does
not properly bundle the metadata from the re-exported genfile.

This causes the bug that we experience when using the Bazel built `@angular/material-examples` package.
