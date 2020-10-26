[//]: # (title: Python)
[//]: # (auxiliary-id: Python)

The Python build runner automatically detects Python on agents and allows running Python scripts on Windows, Linux, and macOS.

>Since TeamCity 2020.2, this bundled runner replaces the obsolete [Python Runner plugin](https://plugins.jetbrains.com/plugin/9042-python-runner). The new runner offers support for virtual environments, Docker, TeamCity Kotlin DSL, and provide more extra features like test coverage.

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [Docker Wrapper](docker-wrapper.md) to learn how you can run this step inside a Docker container.

## Python autodetection

The Python runner automatically detects Python versions installed on a [build agent](build-agent.md).

On Windows, it checks (1) the default install paths, (2) the system register, (3) the `PATH` variable. On Linux and macOS, it checks (1) the default install paths, (2) the `PATH` variable.

The runner sets the first detected versions of Python 2.x and 3.x as the agent's configuration parameters.

## Command settings

You can choose one of the following Python commands:

<table>

<tr>

<td>

Command

</td>

<td>

Description

</td>

<td>

Example input

</td>

</tr>

<tr>

<td>

File

</td>

<td>

Runs a Python script via the provided path.

</td>

<td>

`scripts/test.py`

</td>

</tr>

<tr>

<td>

Module

</td>

<td>

Runs a Python [module](https://docs.python.org/3/tutorial/modules.html) via the provided path or name (equally to adding `-m` before the module name).

</td>

<td>

`scripts/module1.py`

</td>

</tr>

<tr>

<td>

Custom script

</td>

<td>

Allows entering a Python script's body in the UI form.

</td>

<td>

```python
ips = {}

fh = open("/var/log/nginx/access.log", "r").readlines()
for line in fh:
    ip = line.split(" ")[0]
    if 6 < len(ip) <=15:
        ips[ip] = ips.get(ip, 0) + 1
print ips
```

</td>

</tr>

<tr>

<td>

Unittest

</td>

<td>

Launch the [unittest](https://docs.python.org/3/library/unittest.html) framework.

You can specify a path to the unit test file in the additional arguments.

</td>

<td>

Enter `tests/*.py` as an argument to launch all Python files in the `tests` directory via the unittest framework.

</td>

</tr>

<tr>

<td>

Pytest

</td>

<td>

Launch the [pytest](https://docs.pytest.org/en/stable/index.html) framework.

You can specify a path to the pytest file in the additional arguments.

</td>

<td>

Enter `tests/*.py` as an argument to launch all Python files in the `tests` directory via the pytest framework.

</td>

</tr>

<tr>

<td>

Flake8

</td>

<td>

Launch the [Flake8](https://pypi.org/project/flake8/) wrapper.

You can specify a path to the Python file in the additional arguments.

</td>

<td>

Enter `docs\conf.py` as an argument to check the `conf.py` file for errors.

</td>

</tr>

<tr>

<td>

Pylint

</td>

<td>

Launch the [Pylint](https://pypi.org/project/pylint/) tool.

You can specify a path to the Python file in the additional arguments.

</td>

<td>

Enter `scripts\*.py` as an argument to check all Python files in the `scripts` directory for errors.

</td>

</tr>

<tr>

<td>

Arguments

</td>

<td>

Arguments of the interpreter applied in each Python run (for example, `python3 arg1 arg2`).

If the step is launched in a virtual environment, these arguments are applied to the `python` command inside the environment (for example, `python3 -m pipenv run python arg1 arg2`).

</td>

<td>

`arg1 arg2`

</td>

</tr>


</table>

The set of available settings depends on the selected command. This table describes all command settings:

<table>

<tr><td>

Setting

</td><td>

Description

</td></tr>

<tr><td>

File

</td><td>

Path to a Python script file.

</td></tr>

<tr><td>

Module

</td><td>

Path to a Python module file.

</td></tr>

<tr><td>

Script or module arguments

</td><td>

Arguments that will be passed to the user script or module after their call.

</td></tr>

<tr><td>

Script

</td><td>

The custom script body.

</td></tr>

<tr><td>

Test reporting

</td><td>

Enables test reporting via the [`teamcity-messages`](https://pypi.python.org/pypi/teamcity-messages) package.

</td></tr>

<tr><td>

Coverage

</td><td>

Enable code coverage collecting via [Coverage.py](https://coverage.readthedocs.io/en/coverage-5.3/).

</td></tr>

<tr><td>

Python arguments

</td><td>

Arguments that will be passed to the interpreter for the main command run only before the module or script call: for example, `-m {module}`.

</td></tr>


</table>

## Python executable settings

In this block of settings, you can choose a Python version to run. TeamCity can autodetect an installed 2.x or 3.x version, or you can enter a custom path to the Python executable. You can also specify arguments that will be passed to the interpreter in every Python run of this build step (for example, the environment tool run or reporting runs).

## Environment tool settings

Optionally, you can run a Python build step in a virtual environment. The Python runner supports the following tools:
* [Pipenv](https://pipenv.pypa.io/en/latest/)
* [Virtualenv](https://virtualenv.pypa.io/en/latest/)

If you choose Pipenv, enter additional [install arguments](https://pipenv.pypa.io/en/latest/cli/#pipenv-install) if necessary.

Virtualenv has the following settings:

<table>

<tr><td>

Setting

</td><td>

Description

</td></tr>

<tr><td>

Requirement files

</td><td>

Path to the `requirements.txt` file.

</td></tr>

<tr><td>

Pip run arguments

</td><td>

Additional arguments for the `pip run` command.

</td></tr>

<tr><td>

Environment name

</td><td>

Name of the virtual environment.

</td></tr>

<tr><td>

Virtualenv arguments

</td><td>

Additional virtualenv arguments to create a new `env` command.

</td></tr>

</table>
