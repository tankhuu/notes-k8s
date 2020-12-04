# Useful Commands

## Show logs

`kubectl logs mypod --previous`

## Configuring kubectl edit to use a different text editor

You can tell kubectl to use a text editor of your choice by setting the KUBE_EDITOR environment variable. For example, if you’d like to use nano for editing Kubernetes resources, execute the following command (or put it into your ~/.bashrc or an equivalent file):

`export KUBE_EDITOR="/usr/bin/nano"`

If the KUBE_EDITOR environment variable isn’t set, kubectl edit falls back to using the default editor, usually configured through the EDITOR environment variable.
