# openshift-quickstart-sbcl

Quickstart for SBCL on OpenShift (Use hunchentoot as an example).

## Environment

### The Runtime Environment

- [Do-It-Yourself 0.1](https://docs.openshift.org/origin-m4/oo_cartridge_guide.html#diy "DIY")
  on [OpenShift](https://www.openshift.com/ "PaaS by Red Hat, Built on Docker and Kubernetes")

### Requirements / Dependencies

- [Steel Bank Common Lisp (SBCL)](http://sbcl.org/ "A high performance Common Lisp compiler")

- [Quicklisp](https://www.quicklisp.org/ "A library manager for Common Lisp")

- [Another System Definition Facility (ASDF)](https://www.common-lisp.net/project/asdf/ "Another System Definition Facility")

- (*Optional*) [Hunchentoot](http://weitz.de/hunchentoot/ "The Common Lisp web server formerly known as TBNL")

  **You can choose any web server which support SBCL as you like.**

## Usage

Write a file named _build.lisp_ in the root of your source tree to build the
system.

```lisp
(log-title "Building system ...")
(ql:quickload :any-thing-you-want)
(log-footer "")
```

(*Optional*) Write a file named _project.env_ in the root of your source
tree to define environment variables.

```bash
export SBCL_VERSION="sbcl-1.3.5-x86-64-linux"
```

Write a file named _web-launcher.lisp_ in the root of your source tree for
execute when starting the server.

```lisp
(require :asdf)
(asdf:disable-output-translations)
(defvar *virtual-root*
  (pathname (concatenate
             'string (asdf::getenv "VIRTUAL_ROOT") "/")))
(defvar *address* (asdf::getenv "OPENSHIFT_DIY_IP"))
(defvar *port* (parse-integer (asdf::getenv "OPENSHIFT_DIY_PORT")))
(defun require-quicklisp ()
  (let ((quicklisp-init
         (merge-pathnames "quicklisp/setup.lisp" *virtual-root*)))
    (when (probe-file quicklisp-init)
      (load quicklisp-init))))
(require-quicklisp)
(require :web-server)
(web-server:start :address *address* :port *port*)
```

## References

- [DIY/Custom | OpenShift Developers](https://developers.openshift.com/languages/diy.html)

- [Extend OpenShift | OpenShift Developers](https://developers.openshift.com/get-involved/extend-openshift.html)

- [Action Hooks | OpenShift Developers](https://developers.openshift.com/managing-your-applications/action-hooks.html)

- [atgreen/lisp-openshift](https://github.com/atgreen/lisp-openshift)

- [cheryllium/lisp-openshift](https://github.com/cheryllium/lisp-openshift)
