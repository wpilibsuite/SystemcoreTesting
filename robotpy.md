RobotPy 2027
============

## Installation

Install Python, and then:

    python3 -m pip install robotpy~=2027.0.0a7

### Deploy

Update `pyproject.toml` in your robot project with the correct RobotPy version.

The `robotpy sync` and `robotpy deploy` commands should work as normal and
deploy to a Systemcore system. See WPILib documentation for more details.

## Vendor libraries

Vendors have been removed from `robotpy_extras` in `pyproject.toml`. You must add
their pypi package names to the `requires` list instead. The following vendor
packages are known to be available for 2027:

