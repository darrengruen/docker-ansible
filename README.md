# Ansible Docker Container

## What is it

For more information on ansible, ansible-vault, or ansible-playbook go to (ansible.com)[https://www.ansible.com)

## Usage

This container can easily be run with the following commands:

```
docker run -it --rm \
    -v /etc/localtime:/etc/localtime:ro \
    -v [path/to/ansible/files]:/etc/ansible \
    -v [path/to/known/hosts]:/root/.ssh/known_hosts \
    --name ansible_"$(date +%s)" \
    gruen/ansible [ansible|ansible-playbook|ansible-vault] [options]
```

You may run `ansible`, `ansible-playbook`, or `ansible-vault` from this image.

You'll need to mount your ansible file in the container at `/etc/ansible`.\
Optionally you can also specify a directory to store __known hosts__, and mount it in the container at `/root/.ssh/known_hosts`.

The `--name ansible_"$(date +%s)"` is a convention I use.
This allows me to run multiple instances of the same container without name collisions and a formatted name.

For ease of use, create a few shell functions or scripts similar to the following:

```
ansible() {
    command="${1:-ansible}"

    [ "${1}" ] && shift

    docker run -it --rm \
        -v /etc/localtime:/etc/localtime:ro \
        -v [path/to/ansible/files]:/etc/ansible \
        -v [path/to/known/hosts/files:/root/.ssh/known_hosts \
        -- name ansible_"$(date +%s)" \
        gruen/ansible "${command}" "${@}"
}
ansible-playbook() {
    ansible ansible-playbook "${@}"
}
ansible-vault() {
    ansible ansible-vault "${@}"
}
```
