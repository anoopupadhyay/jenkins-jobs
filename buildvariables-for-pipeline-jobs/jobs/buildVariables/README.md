# README.md buildVariables

## Description

Creates a buildVariable object for Pipeline jobs.

## Details

A Freestyle and Maven project both have a buildVariable object created automatically which contains
the environment variables for the build. The buildVariable object doesn't get created for Pipeline jobs.
This Pipeline job creates a buildVariable. This script should be made available in a shared library.
