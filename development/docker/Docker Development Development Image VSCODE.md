
# devcontainer.runArgs

devcontainer runArgs are persisted when the container is built from the image... but changes should be detected so you can choose rebuild the image

How to default to the current user because we didn't add a vscode user.
Turns out we need to use the vscode user name mapped to our user PID without changing the name so it's predictable.

In the docker file can you override ARGS that have a value? by passing --build-arg USER_NAME=user
ARGS USER_NAME=vscode

We shall see? Yes it seems to be... how do we prevent that... maybe use ENV instead? Whatver just don't do it.

