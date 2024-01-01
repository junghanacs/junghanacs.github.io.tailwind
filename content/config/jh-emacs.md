+++
title = "jh-emacs configuration"
author = ["Junghan Kim"]
series = ["Emacs Guide"]
categories = ["Emacs"]
draft = false
+++

이것은 제가 작성한 Emacs 구성 파일입니다. 산문과 코드의 조합입니다. 이 페이지를
읽거나 제 닷파일을 확인하여 제 Emacs 설정과 관련된 모든 것을 찾을 수 있습니다.

<!--more-->

> Currently tailored for GNU Emacs 29.1

**Last revised and exported on 2023-12-30 18:14:27 +0900 with a word
count of 88189.**


## <span class="section-num">1</span> Introduction {#h:920df469-bf7a-4cd0-adf5-d1da73d74189}

아래는 샘플이다. 이제 서문을 쓸 때가 되었다. 아래 샘플이 있다. 거의 다를게 없다.

```text
sqrt-dotfiles/Emacs.org:13
```

My configuration of [GNU Emacs](https://www.gnu.org/software/emacs/), an awesome ~~text editor~~ program that can do almost
anything.

At the moment of this writing, this "almost anything" includes:

-   **Writing code**. With LSP &amp; Co Emacs is as good as many IDEs, and is certainly
    on par with editors like VS Code.<br />
    Emacs is also particularly good at writing Lisp code, e.g. Clojure, Common
    Lisp, and, of course, Emacs Lisp.
-   **Literate programming** with Org Mode. That includes:
    -   Configuring the entirety of my software (that can be configured with text
        files).
    -   Interactive programming like one provided by Jupyter Notebook.
-   **File management**. Dired is my primary file manager.
-   **Email**, with notmuch.
-   **Multimedia management**, with EMMS.
-   **RSS feed reader**, with elfeed.
-   **Task management**, with Org Mode.
-   **Managing passwords**, with pass.
-   **IRC**, with ERC.
-   **Formatting documents**, also with Org Mode. When the document is too complex,
    I prefer to write plain LaTeX, but I've come to the conclusion that in most
    cases Org Mode covers my needs there.
-   **X Window management**, with EXWM. So I could say I literally live in Emacs.
-   ...

As I have hinted above, this file is a piece of literate configuration, where
the actual code is interweaved with English-language commentary. One could argue
that the commentary, and not the code, is the primary entity of the file.

But at the same time, the configuration is personal, so the primary benefactor
of the literate structure is me. The commentary is primarily meant to capture my
state of mind at the moment of writing the code, which is immensely helpful for
maintaining the code in the future. So the quality and quantity of the
commentary are... varying.

Occasionally I save some promising experimentations from scratch buffers without
much comment. Or I may not have enough time to describe things in substantial
detail. Or, as it is at the moment when I'm writing this, I have the time to
write down whatever I consider necessary. Plus I usually incorporate my blog
posts back into the config.

Of course, human minds share many similarities, so if you are an avid Emacs
user, you have a chance to extract something of value from this document.

If however, by some twist of fate, this document is one of the first things you
see about Emacs, it won't be a good resource for you. And you definitely
shouldn't try to launch this config as it is. If I could suggest only one
resource, I'd advise David Wilson's [System Crafters](https://www.youtube.com/c/SystemCrafters) YouTube channel.



```org
*한영 자간 확인*
+------------+------------+
| 일이삼사오 | 일이삼사오 |
| abcdefghij | abcdefghij |
+------------+------------+
```


## <span class="section-num">2</span> <kbd>Spacemacs</kbd> Configurations (`init.el`) {#h:dae63bd9-93a8-41c4-af1b-d0f39ba50974}


### <span class="section-num">2.1</span> Reproducible information {#h:63e216e8-6d56-47cc-bb68-d87cf988d9b5}

This configuration is continuingly being improved. I build my own Emacs from
source in order to take advantage of some experimental features. There are also
`(packages! ...)` calls to external Emacs packages that are unpinned to any
specific version. As such, there might be incompabilities if one blindly copies
codes from this configurations. Although I'll try to document which features are
based on developing softwares and are likely to be changed in the future, it is
inevitable that some bits of information are going to fall through the cracks.

In this section, I reiterate the relevant info about the version of the software
I'm using here, in case someone finds this infomation useful. Here's my current
build of Emacs:

```emacs-lisp
(emacs-version)
```

```text
GNU Emacs 29.1.50 (build 2, x86_64-pc-linux-gnu, GTK+ Version 3.24.37, cairo version 1.16.0)
 of 2023-09-13
```

This Emacs is built with the following configuration options:

```emacs-lisp
system-configuration-options
```

```text
--with-native-compilation --with-json --without-pop --with-gnutls --without-mailutils --with-sqlite3 --with-rsvg --with-png --with-jpeg --with-tiff --with-imagemagick --with-tree-sitter --with-cairo --with-lcms2 --with-modules --with-xwidgets --with-x-toolkit=gtk3 '--program-transform-name=s/^ctags$/ctags.emacs/' 'CFLAGS=-O2 -pipe -mtune=native -march=native -fomit-frame-pointer'
```


### <span class="section-num">2.2</span> Headers init.el {#h:8501ac0f-3033-43ca-a000-3bff6fe15709}

This generates the top of the init file, which will set up the lexical scope and describe to Emacs what the file does.

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3
;;
;; This program is free software: you can redistribute it and/or modify it under
;; the terms of the GNU General Public License as published by the Free Software
;; Foundation, either version 3 of the License, or (at your option) any later
;; version.
;;
;; This program is distributed in the hope that it will be useful, but
;; WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
;; FITNESS FOR A PARTICULAR PURPOSE.
;;
;; See the GNU General Public License for more details. You should have received
;; a copy of the GNU General Public License along with this program. If not, see
;; <https://www.gnu.org/licenses/>.
;;
;;  ____
;; / ___| _ __   __ _  ___ ___ _ __ ___   __ _  ___ ___
;; \___ \| '_ \ / _` |/ __/ _ \ '_ ` _ \ / _` |/ __/ __|
;;  ___) | |_) | (_| | (_|  __/ | | | | | (_| | (__\__ \
;; |____/| .__/ \__,_|\___\___|_| |_| |_|\__,_|\___|___/
;;       |_|
;;
;;; Commentary:

;;  While any text editor can save your files, only Emacs can save your soul
;; ~/sync/org/roam/configs/spacemacs.org

;; fix Emacs 30.x on Android ELPA gpg problem
;; $ gpg --homedir ~/.emacs.d/elpa/gnupg --receive-keys 066DAFCB81E42C40

;; ❶ :: U+2776 ==> 더원싱 태그로 활용
;; ㉽ :: U+327D
;; ㉼ :: U+327C
```


### <span class="section-num">2.3</span> Pre-Init and Load {#h:1fb6ee86-805f-4cc5-8f22-6fadd9e69af2}

```elisp

;;; Pre-Init and Load

(setq spacemacs-buffer-logo-title "[J U N G H A N A C S]")

;; (setq debug-on-error t)

;;;; Load paths

;; ;; Recommended to have this at the top
;; (setq treesit-extra-load-path `(,(concat user-emacs-directory "tree-sitter-module/dist/")
;;                                 ,(concat user-emacs-directory "tree-sitter")))

;; optimize: force "lisp"" and "site-lisp" at the head to reduce the startup time.
;; (add-to-list 'load-path (concat dotspacemacs-directory "lisp"))
(dolist (dir '("site-lisp" "lisp" "menu" ))
  (push (expand-file-name dir dotspacemacs-directory) load-path))

;;;; Config - Frame Version PGTK

;; You should be able to use input methods since GtkIMContext is enabled by default.
;; If you don't like GtkIMContext, you can disable it by writing as follows in ~/.emacs:
;; pgtk-use-im-context
;; disable gtk im modules for emacs-pgtk, add "Emacs*UseXIM: false" to ~/.Xresources to disable xim
(if (eq window-system 'pgtk)
    (pgtk-use-im-context nil))

(when (boundp 'pgtk-use-im-context-on-new-connection)
  (setq pgtk-use-im-context-on-new-connection nil))

;; Emacs version 29 added a new frame parameter for "true" transparency, which
;; means that only the blackground is transparent while the text is not.
;; started to use new #emacs 29 alpha-background frame-parameters. It only works on
;; gnu/#linux at the moment and look beautiful :

(if (eq system-type 'gnu/linux)
    (setq default-frame-alist (push '(alpha-background . 93) default-frame-alist))
  (setq default-frame-alist (push '(alpha . (95 90)) default-frame-alist)))

(setq emacs-major-version-string (format "%s" emacs-major-version))

;; (unless (display-graphic-p) ; terminal
;;   (set-display-table-slot standard-display-table
;;                           'vertical-border
;;                           (make-glyph-code ?│)))

;;;; Custom Functions

;; (defun delete-nth (index seq)
;;   "Delete the INDEX th element of SEQ.
;;    Return result sequence, SEQ __is__ modified."
;;   (if (equal index 0)
;;     (progn
;;       (setcar seq (car (cdr seq)))
;;       (setcdr seq (cdr (cdr seq))))
;;     (setcdr (nthcdr (1- index) seq) (nthcdr (1+ index) seq))))

;; (defun set-nth (index seq newval)
;;   "Set the INDEX th element of SEQ to NEWVAL.
;;    SEQ __is__ modified."
;;   (setcar (nthcdr index seq) newval))

;;;; Emacs-startup-hook

(setq my/emacs-started nil)

(add-hook 'emacs-startup-hook
          (lambda ()
            (message "*** Emacs loaded in %s with %d garbage collections."
                     (format "%.2f seconds"
                             (float-time
                              (time-subtract after-init-time before-init-time)))
                     gcs-done)
            (setq my/emacs-started t)))

;;;; Check External Tools

;; Check exernal tools
(defun bool (val) (not (null val)))
;; Some packages do not work correctly on Emacs built with the LUCID feature
(defconst AG-P (bool (executable-find "ag")))
(defconst MPD-P (bool (and (executable-find "mpc") (executable-find "mpd"))))
(defconst MPV-P (bool (executable-find "mpv")))
(defconst REPO-P (bool (executable-find "repo")))
(defconst ZOTERO-P (bool (executable-find "zotero")))

;;;; Custom Define 'emacs-type'

(setq-default root-path "/")

(defcustom emacs-type 'spacemacs
  "Select Emacs Distribution Types"
  :group 'emacs
  :type  '(choice
           (const :tag "spacemacs" spacemacs)
           (const :tag "minemacs" minemacs)
           (const :tag "doomemacs" doomemacs)
           (const :tag "vanillaemacs" vanillaemacs)))

(defun is-spacemacs() (eq emacs-type 'spacemacs))
(defun is-minemacs() (eq emacs-type 'minemacs))
(defun is-doomemacs() (eq emacs-type 'doomemacs))
(defun is-vanillaemacs() (eq emacs-type 'vanillaemacs))

;; (when (is-spacemacs) (message "I Love Spacemacs"))

;; /home/junghan/sync/man/dotsamples/korean/injae-dotfiles/module/+emacs.el
(defvar *is-mac*     (eq system-type 'darwin))
(defvar *is-windows* (eq system-type 'windows-nt))
(defvar *is-cygwin*  (eq system-type 'cygwin))
(defvar *is-linux*   (or (eq system-type 'gnu/linux) (eq system-type 'linux)))
(defvar *is-wsl*     (eq (string-match "Linux.*microsoft.*WSL2.*Linux" (shell-command-to-string "uname -a")) 0))
(defvar *is-unix*    (or *is-linux* (eq system-type 'usg-unix-v) (eq system-type 'berkeley-unix)))
(defvar *is-android*  (eq system-type 'android)) ; android native emacs

;; (defvar *is-termux*
;;   (and (eq system-type 'gnu/linux)
;;     (string-suffix-p "Android" (string-trim (shell-command-to-string "uname -a")))))

;; on anroid 는 모두 해당
(defvar *is-termux*
  (string-suffix-p "Android" (string-trim (shell-command-to-string "uname -a"))))

(when *is-termux*
  (setq root-path "/data/data/com.termux/files/"))

(setq my/slow-ssh
      (or
       (string= (getenv "IS_TRAMP") "true")))

(setq my/remote-server
      (or (string= (getenv "IS_REMOTE") "true")
          ;; (string-suffix-p "Android" (string-trim (shell-command-to-string "uname -a")))
          (string= (system-name) "server1")
          (string= (system-name) "server2")
          (string= (system-name) "server3"))) ; for test

(setenv "IS_EMACS" "true")

;;;; is-android gui

(when *is-android*
  (message "Loading Android Emacs\n")
  (setenv "PATH" (format "%s:%s" "/data/data/com.termux/files/usr/bin" (getenv "PATH")))
  (setenv "LD_LIBRARY_PATH" (format "%s:%s" "/data/data/com.termux/files/usr/lib" (getenv "LD_LIBRARY_PATH")))
  (push "/data/data/com.termux/files/usr/bin" exec-path))

```


### <span class="section-num">2.4</span> Spacemacs Layer {#h:9b08d43c-d97b-415c-adf7-2f7b4661de40}

```elisp
;;; Spacemacs Layer

(defun dotspacemacs/layers ()

;;;; 'Base' Configurations

  (setq-default
   ;; Base distribution to use. This is a layer contained in the directory
   ;; `+distribution'. For now available distributions are `spacemacs-base'
   ;; or `spacemacs'. (default 'spacemacs)
   dotspacemacs-distribution 'spacemacs-base
   ;; 명시적으로 아래 정의한 패키지만 사용한다.
   ;; Lazy installation of layers (i.e. layers are installed only when a file
   ;; with a supported type is opened). Possible values are `all', `unused'
   ;; and `nil'. `unused' will lazy install only unused layers (i.e. layers
   ;; not listed in variable `dotspacemacs-configuration-layers'), `all' will
   ;; lazy install any layer that support lazy installation even the layers
   ;; listed in `dotspacemacs-configuration-layers'. `nil' disable the lazy
   ;; installation feature and you have to explicitly list a layer in the
   ;; variable `dotspacemacs-configuration-layers' to install it.
   ;; (default 'unused)
   dotspacemacs-enable-lazy-installation nil
   ;; dotspacemacs-enable-lazy-installation 'unused
   dotspacemacs-ask-for-lazy-installation t
   dotspacemacs-configuration-layer-path `(,(expand-file-name "layers/" dotspacemacs-directory))
   dotspacemacs-directory-snippets-dir '(concat doctspacemacs-directory "snippets/"))

;;;; 'Layer' Declarations

  ;; Default Layer Configurations
  ;; List of configuration layers to load.
  (setq-default
   dotspacemacs-configuration-layers
   '(
     jh-base
     jh-completion
     jh-visual
     jh-workspace
     jh-checker
     jh-writing
     jh-reading
     jh-coding
     jh-org
     jh-misc
     ))

  ;; Load custom-layer-filename
  (setq custom-layer-filename (concat dotspacemacs-directory "layers/load-" emacs-major-version-string ".el"))
  (when (file-exists-p custom-layer-filename)
    (load-file custom-layer-filename))

  ;; Load custom-layer-filename
  ;; (let ((custom-layer-filename (concat dotspacemacs-directory "layers/load-" emacs-major-version-string ".el")))
  ;;   (when (file-exists-p custom-layer-filename)
  ;;     (load-file custom-layer-filename)))

;;;; 'Extra' Package Options

  ;; List of additional packages that will be installed without being wrapped
  ;; in a layer (generally the packages are installed only and should still be
  ;; loaded using load/require/use-package in the user-config section below in
  ;; this file). If you need some configuration for these packages, then
  ;; consider creating a layer. You can also put the configuration in
  ;; `dotspacemacs/user-config'. To use a local version of a package, use the
  ;; `:location' property: '(your-package :location "~/path/to/your-package/")
  ;; Also include the dependencies as they will not be resolved automatically.
  (setq-default
   dotspacemacs-additional-packages
   '(
     ;; musicbrainz ; music database api

     ;; etherpad ; 오픈소스 온라인 공동 편집 - 이맥스 연동
     ;; i-ching ; 주역 또는 역경은 점술 방법, 패턴 생성기 ?
     ;; smog ; A simple way to analyse the writing style, word use and readability of prose in Emacs.
     ;; quiet ; disconnect from the online world for a while

     triples
     ekg
     llm

     ellama
     gptel

     immersive-translate

     pretty-hydra

     ;; revert-buffer-all
     ;; docsim ; search document with syntax

     ;; (spookfox :location (recipe :fetcher github :repo "bitspook/spookfox"
     ;;                       :files ("lisp/*.el" "lisp/apps/*.el")))

     ;; (link-hint-aw-select :location (recipe :fetcher github :repo "localauthor/link-hint-aw-select"))
     ;; (zk :location (recipe :fetcher github :repo "junghan0611/forked-zk" :files ("*.el" "*.org")))
     ;; (zk-luhmann :location (recipe :fetcher github :repo "junghan0611/zk-luhmann" :branch "ko" :files ("*.el" "*.org")))
     ;; (link-hint-preview :location (recipe :fetcher github :repo "localauthor/link-hint-preview"))
     ;; (link-preview :location (recipe :fetcher github :repo "aviaviavi/link-preview.el"))

     ;; (org-protocol-capture-html :location (recipe :fetcher github :repo "alphapapa/org-protocol-capture-html"))

     ;; (hyperbole :location (recipe :fetcher github :repo "rswgnu/hyperbole"))

     ;; (champagne :location (recipe :fetcher github
     ;;                              :repo "positron-solutions/champagne"))
     ;; (transient-showcase :location
     ;;                     (recipe :fetcher github
     ;;                             :repo "positron-solutions/transient-showcase"))

     ;; (ox-moderncv :location (recipe :fetcher github :repo "ohyecloudy/org-cv"))
     ;; (org-auctex :location (recipe :fetcher github "karthink/org-auctex"))

     ;; (cal-korea-x :location (recipe :fetcher github :repo "cinsk/cal-korea-x"))
     ;; (typo :location (recipe :fetcher sourcehut :repo "pkal/typo")) ; TODO CHECK

     ;; ox-zenn
     ;; ox-qmd
     )
   )

  ;; emacs-verson >= 30
  ;; (when (version< "30" emacs-version)
  ;; (when (>= emacs-major-version 30)
  ;;   (add-to-list 'dotspacemacs-additional-packages
  ;;     '(compat :location (recipe :fetcher github
  ;;                          :repo "junghan0611/compat")))
  ;;   ;; Get some Emacs 29 compatibility functions. Notably missing is
  ;;   ;; `setopt' which the `compat' library deliberately does not
  ;;   ;; provide, so we continue to use the `customize-set-variable'
  ;;   ;; function for setting user options, unless we have a version guard
  ;;   ;; around a block, in which case we use `setopt' instead.
  ;;   ;; (package-vc-install "https://github.com/junghan0611/compat")
  ;;   )

  (setq-default
   dotspacemacs-frozen-packages '()
   dotspacemacs-excluded-packages'(
                                   alchemist
                                   company
                                   auto-complete ac-ispell
                                   tern
                                   flycheck-pos-tip
                                   web-beautify
                                   emoji-cheat-sheet-plus ; dependent helm
                                   counsel-gtags
                                   fancy-battery
                                   fish-mode
                                   valign
                                   undo-tree
                                   volatile-highlights
                                   )
   dotspacemacs-install-packages 'used-only)
  ) ; defun dotspacemacs/layers

```


### <span class="section-num">2.5</span> Spacemacs Configuration {#h:6a6a1910-8881-4d0c-aed7-93539d563f92}

```elisp
;;; Spacemacs Configuration

(defun dotspacemacs/init ()

;;;; Start and several functions

  ;; This setq-default sexp is an exhaustive list of all the supported
  ;; spacemacs settings.
  (setq-default
   ;; If non-nil then enable support for the portable dumper. You'll need to
   ;; compile Emacs 27 from source following the instructions in file
   ;; EXPERIMENTAL.org at to root of the git repository.
   ;;
   ;; WARNING: pdumper does not work with Native Compilation, so it's disabled
   ;; regardless of the following setting when native compilation is in effect.
   ;;
   ;; (default nil)
   dotspacemacs-enable-emacs-pdumper nil

   ;; Name of executable file pointing to emacs 27+. This executable must be
   ;; in your PATH.
   ;; (default "emacs")
   dotspacemacs-emacs-pdumper-executable-file "emacs"

   ;; Name of the Spacemacs dump file. This is the file will be created by the
   ;; portable dumper in the cache directory under dumps sub-directory.
   ;; To load it when starting Emacs add the parameter `--dump-file'
   ;; when invoking Emacs 27.1 executable on the command line, for instance:
   ;;   ./emacs --dump-file=$HOME/.emacs.d/.cache/dumps/spacemacs-27.1.pdmp
   ;; (default (format "spacemacs-%s.pdmp" emacs-version))
   dotspacemacs-emacs-dumper-dump-file (format "spacemacs-%s.pdmp" emacs-version)

   ;; If non-nil ELPA repositories are contacted via HTTPS whenever it's
   ;; possible. Set it to nil if you have no way to use HTTPS in your
   ;; environment, otherwise it is strongly recommended to let it set to t.
   ;; This variable has no effect if Emacs is launched with the parameter
   ;; `--insecure' which forces the value of this variable to nil.
   ;; (default t)
   dotspacemacs-elpa-https t

   ;; Maximum allowed time in seconds to contact an ELPA repository.
   ;; (default 5)
   dotspacemacs-elpa-timeout 15

   ;; Set `gc-cons-threshold' and `gc-cons-percentage' when startup finishes.
   ;; This is an advanced option and should not be changed unless you suspect
   ;; performance issues due to garbage collection operations.
   ;; (default '(100000000 0.1))
   dotspacemacs-gc-cons '(100000000 0.1)
   ;; dotspacemacs-gc-cons '(256000000 0.1)

   ;; Set `read-process-output-max' when startup finishes.
   ;; This defines how much data is read from a foreign process.
   ;; Setting this >= 1 MB should increase performance for lsp servers
   ;; in emacs 27.
   ;; (default (* 1024 1024))
   dotspacemacs-read-process-output-max (* 1024 1024)

   ;; If non-nil then Spacelpa repository is the primary source to install
   ;; a locked version of packages. If nil then Spacemacs will install the
   ;; latest version of packages from MELPA. Spacelpa is currently in
   ;; experimental state please use only for testing purposes.
   ;; (default nil)
   dotspacemacs-use-spacelpa nil

   ;; If non-nil then verify the signature for downloaded Spacelpa archives.
   ;; (default t)
   dotspacemacs-verify-spacelpa-archives t

   ;; If non-nil then spacemacs will check for updates at startup
   ;; when the current branch is not `develop'. Note that checking for
   ;; new versions works via git commands, thus it calls GitHub services
   ;; whenever you start Emacs. (default nil)
   dotspacemacs-check-for-update nil

   ;; If non-nil, a form that evaluates to a package directory. For example, to
   ;; use different package directories for different Emacs versions, set this
   ;; to `emacs-version'. (default 'emacs-version)
   dotspacemacs-elpa-subdirectory 'emacs-version

   ;; One of `vim', `emacs' or `hybrid'.
   ;; `hybrid' is like `vim' except that `insert state' is replaced by the
   ;; `hybrid state' with `emacs' key bindings. The value can also be a list
   ;; with `:variables' keyword (similar to layers). Check the editing styles
   ;; section of the documentation for details on available variables.
   ;; (default 'vim)
   dotspacemacs-editing-style 'vim
   ;; dotspacemacs-editing-style '(vim :variables
   ;;                                  vim-style-visual-feedback t ; default nil
   ;;                                  vim-style-remap-Y-to-y$ nil
   ;;                                  vim-style-retain-visual-state-on-shift t
   ;;                                  vim-style-visual-line-move-text t  ; default nil
   ;;                                  vim-style-ex-substitute-global nil)

   ;; If non-nil show the version string in the Spacemacs buffer. It will
   ;; appear as (spacemacs version)@(emacs version)
   ;; (default t)
   dotspacemacs-startup-buffer-show-version t

   ;; Specify the startup banner. Default value is `official', it displays
   ;; the official spacemacs logo. An integer value is the index of text
   ;; banner, `random' chooses a random text banner in `core/banners'
   ;; directory. A string value must be a path to an image format supported
   ;; by your Emacs build.
   ;; If the value is nil then no banner is displayed. (default 'official)
   dotspacemacs-startup-banner 100 ; 'random
   ;; dotspacemacs-startup-banner (concat
   ;;                              dotspacemacs-directory "assets/splash/emacs.txt")

   ;; Scale factor controls the scaling (size) of the startup banner. Default
   ;; value is `auto' for scaling the logo automatically to fit all buffer
   ;; contents, to a maximum of the full image height and a minimum of 3 line
   ;; heights. If set to a number (int or float) it is used as a constant
   ;; scaling factor for the default logo size.
   dotspacemacs-startup-banner-scale 'auto

   ;; List of items to show in startup buffer or an association list of
   ;; the form `(list-type . list-size)`. If nil then it is disabled.
   ;; Possible values for list-type are:
   ;; `recents' `recents-by-project' `bookmarks' `projects' `agenda' `todos'.
   ;; List sizes may be nil, in which case
   ;; `spacemacs-buffer-startup-lists-length' takes effect.
   ;; The exceptional case is `recents-by-project', where list-type must be a
   ;; pair of numbers, e.g. `(recents-by-project . (7 .  5))', where the first
   ;; number is the project limit and the second the limit on the recent files
   ;; within a project.
   ;; dotspacemacs-startup-lists '(
   ;;                              (projects . 5)
   ;;                              ;; (agenda . 5)
   ;;                              (bookmarks . 5)
   ;;                              ;; (recents . 5)
   ;;                              )
   dotspacemacs-startup-lists nil

   ;; True if the home buffer should respond to resize events. (default t)
   dotspacemacs-startup-buffer-responsive t

   ;; Show numbers before the startup list lines. (default t)
   dotspacemacs-show-startup-list-numbers nil

   ;; The minimum delay in seconds between number key presses. (default 0.4)
   dotspacemacs-startup-buffer-multi-digit-delay 0.4

   ;; If non-nil, show file icons for entries and headings on Spacemacs home buffer.
   ;; This has no effect in terminal or if "all-the-icons" package or the font
   ;; is not installed. (default nil)
   dotspacemacs-startup-buffer-show-icons nil

   ;; Default major mode for a new empty buffer. Possible values are mode
   ;; names such as `text-mode'; and `nil' to use Fundamental mode.
   dotspacemacs-new-empty-buffer-major-mode 'text-mode

   ;; Default major mode of the scratch buffer (default `text-mode')
   dotspacemacs-scratch-mode 'emacs-lisp-mode

   ;; If non-nil, *scratch* buffer will be persistent. Things you write down in
   ;; *scratch* buffer will be saved and restored automatically.
   dotspacemacs-scratch-buffer-persistent t

   ;; If non-nil, `kill-buffer' on *scratch* buffer
   ;; will bury it instead of killing.
   dotspacemacs-scratch-buffer-unkillable t

   ;; Initial message in the scratch buffer, such as "Welcome to Spacemacs!"
   dotspacemacs-initial-scratch-message ";; Welcome to Junghanacs!"

;;;; Configuration

   dotspacemacs-mode-line-theme '(spacemacs :separator zigzag :separator-scale 1.5)
   ;; dotspacemacs-mode-line-theme '(doom)

   ;; Default font or prioritized list of fonts. The `:size' can be specified as
   ;; a non-negative integer (pixel size), or a floating-point (point size).
   ;; Point size is recommended, because it's device independent. (default 10.0)
   ;; dotspacemacs-default-font '("Sarasa Mono K"
   ;;                             :size 13.5 ; 13.5, 15.0
   ;;                             :width normal
   ;;                             :weight regular)

   ;; If non-nil the cursor color matches the state color in GUI Emacs.
   dotspacemacs-colorize-cursor-according-to-state t

   dotspacemacs-leader-key "SPC"

   ;; The key used for Emacs commands `M-x' (after pressing on the leader key).
   dotspacemacs-emacs-command-key "SPC"

   ;; The key used for Vim Ex commands (default ":")
   dotspacemacs-ex-command-key ":"

   ;; The leader key accessible in `emacs state' and `insert state'
   ;; (default "M-m")
   dotspacemacs-emacs-leader-key "M-m"

   ;; Major mode leader key is a shortcut key which is the equivalent of
   ;; pressing `<leader> m`. Set it to `nil` to disable it. (default ",")
   dotspacemacs-major-mode-leader-key ","

   ;; Major mode leader key accessible in `emacs state' and `insert state'.
   ;; (default "C-M-m" for terminal mode, "<M-return>" for GUI mode).
   ;; Thus M-RET should work as leader key in both GUI and terminal modes.
   ;; C-M-m also should work in terminal mode, but not in GUI mode.
   dotspacemacs-major-mode-emacs-leader-key (if window-system "<M-return>" "C-M-m")

   ;; These variables control whether separate commands are bound in the GUI to
   ;; the key pairs `C-i', `TAB' and `C-m', `RET'.
   ;; Setting it to a non-nil value, allows for separate commands under `C-i'
   ;; and TAB or `C-m' and `RET'.
   ;; In the terminal, these pairs are generally indistinguishable, so this only
   ;; works in the GUI. (default nil)
   dotspacemacs-distinguish-gui-tab t ; t if evil-better-jumper layer

   ;; Name of the default layout (default "Default")
   dotspacemacs-default-layout-name "Default"

   ;; If non-nil the default layout name is displayed in the mode-line.
   ;; (default nil)
   dotspacemacs-display-default-layout t

   ;; If non-nil then the last auto saved layouts are resumed automatically upon
   ;; start. (default nil)
   dotspacemacs-auto-resume-layouts nil

   ;; If non-nil, auto-generate layout name when creating new layouts. Only has
   ;; effect when using the "jump to layout by number" commands. (default nil)
   dotspacemacs-auto-generate-layout-names nil

   ;; Size (in MB) above which spacemacs will prompt to open the large file
   ;; literally to avoid performance issues. Opening a file literally means that
   ;; no major mode or minor modes are active. (default is 1)
   dotspacemacs-large-file-size 5

   ;; Location where to auto-save files. Possible values are `original' to
   ;; auto-save the file in-place, `cache' to auto-save the file to another
   ;; file stored in the cache directory and `nil' to disable auto-saving.
   ;; (default 'cache)
   dotspacemacs-auto-save-file-location 'cache

   ;; Maximum number of rollback slots to keep in the cache. (default 5)
   dotspacemacs-max-rollback-slots 5

   ;; If non-nil, the paste transient-state is enabled. While enabled, after you
   ;; paste something, pressing `C-j' and `C-k' several times cycles through the
   ;; elements in the `kill-ring'. (default nil)
   dotspacemacs-enable-paste-transient-state nil

   ;; Which-key delay in seconds. The which-key buffer is the popup listing
   ;; the commands bound to the current keystroke sequence. (default 0.4)
   dotspacemacs-which-key-delay 0.4

   ;; Which-key frame position. Possible values are `right', `bottom' and
   ;; `right-then-bottom'. right-then-bottom tries to display the frame to the
   ;; right; if there is insufficient space it displays it at the bottom.
   ;; (default 'bottom)
   dotspacemacs-which-key-position 'bottom

   ;; Control where `switch-to-buffer' displays the buffer. If nil,
   ;; `switch-to-buffer' displays the buffer in the current window even if
   ;; another same-purpose window is available. If non-nil, `switch-to-buffer'
   ;; displays the buffer in a same-purpose window even if the buffer can be
   ;; displayed in the current window. (default nil)
   dotspacemacs-switch-to-buffer-prefers-purpose t

   ;; If non-nil a progress bar is displayed when spacemacs is loading. This
   ;; may increase the boot time on some systems and emacs builds, set it to
   ;; nil to boost the loading time. (default t)
   dotspacemacs-loading-progress-bar nil

   ;; If non-nil the frame is fullscreen when Emacs starts up. (default nil)
   dotspacemacs-fullscreen-at-startup nil

   ;; If non-nil `spacemacs/toggle-fullscreen' will not use native fullscreen.
   ;; Use to disable fullscreen animations in OSX. (default nil)
   dotspacemacs-fullscreen-use-non-native nil

   ;; If non-nil the frame is maximized when Emacs starts up.
   ;; Takes effect only if `dotspacemacs-fullscreen-at-startup' is nil.
   dotspacemacs-maximized-at-startup nil

   ;; If non-nil the frame is undecorated when Emacs starts up. Combine this
   ;; variable with `dotspacemacs-maximized-at-startup' in OSX to obtain
   ;; borderless fullscreen. (default nil)
   dotspacemacs-undecorated-at-startup nil

   ;; A value from the range (0..100), in increasing opacity, which describes
   ;; the transparency level of a frame when it's active or selected.
   ;; Transparency can be toggled through `toggle-transparency'. (default 90)
   dotspacemacs-active-transparency 90

   ;; A value from the range (0..100), in increasing opacity, which describes
   ;; the transparency level of a frame when it's inactive or deselected.
   ;; Transparency can be toggled through `toggle-transparency'. (default 90)
   dotspacemacs-inactive-transparency 90

   ;; If non-nil show the titles of transient states. (default t)
   dotspacemacs-show-transient-state-title t

   ;; If non-nil show the color guide hint for transient state keys. (default t)
   dotspacemacs-show-transient-state-color-guide t

   ;; If non-nil unicode symbols are displayed in the mode line.
   ;; If you use Emacs as a daemon and wants unicode characters only in GUI set
   ;; the value to quoted `display-graphic-p'. (default t)
   dotspacemacs-mode-line-unicode-symbols nil

   ;; If non-nil smooth scrolling (native-scrolling) is enabled. Smooth
   ;; scrolling overrides the default behavior of Emacs which recenters point
   ;; when it reaches the top or bottom of the screen. (default t)
   dotspacemacs-smooth-scrolling t

   ;; Show the scroll bar while scrolling. The auto hide time can be configured
   ;; by setting this variable to a number. (default t)
   dotspacemacs-scroll-bar-while-scrolling nil

   ;; Control line numbers activation.
   ;; If set to `t', `relative' or `visual' then line numbers are enabled in all
   ;; `prog-mode' and `text-mode' derivatives. If set to `relative', line
   ;; numbers are relative. If set to `visual', line numbers are also relative,
   ;; but only visual lines are counted. For example, folded lines will not be
   ;; counted and wrapped lines are counted as multiple lines.
   ;; This variable can also be set to a property list for finer control:
   ;; When used in a plist, `visual' takes precedence over `relative'.
   ;; dotspacemacs-line-numbers '(:relative t
   dotspacemacs-line-numbers '(:relative t
                                         :disabled-for-modes dired-mode
                                         text-mode ; for performance issue
                                         ;; org-mode
                                         doc-view-mode
                                         pdf-view-mode
                                         :size-limit-kb 1000)

   ;; Code folding method. Possible values are `evil', `origami' and `vimish'.
   ;; (default 'evil)
   dotspacemacs-folding-method 'evil

   ;; If non-nil and `dotspacemacs-activate-smartparens-mode' is also non-nil,
   ;; `smartparens-strict-mode' will be enabled in programming modes.
   ;; (default nil)
   ;; 2023-07-31 global 로 켜면 evil 과 충돌. 이거 끄고 글로벌 켜라
   dotspacemacs-smartparens-strict-mode nil ; vs. 'puni' + 'electric-pair'

   ;; If non-nil smartparens-mode will be enabled in programming modes.
   ;; (default t)
   dotspacemacs-activate-smartparens-mode nil ; vs. use 'puni'

   ;; If non-nil pressing the closing parenthesis `)' key in insert mode passes
   ;; over any automatically added closing parenthesis, bracket, quote, etc...
   ;; This can be temporary disabled by pressing `C-q' before `)'. (default nil)
   dotspacemacs-smart-closing-parenthesis nil ; conflict vterm ')'

   ;; Select a scope to highlight delimiters. Possible values are `any',
   ;; `current', `all' or `nil'. Default is `all' (highlight any scope and
   ;; emphasis the current one). (default 'all)
   dotspacemacs-highlight-delimiters 'all

   ;; If non-nil, start an Emacs server if one is not already running.
   ;; (default nil)
   dotspacemacs-enable-server t

   ;; Set the emacs server socket location.
   ;; If nil, uses whatever the Emacs default is, otherwise a directory path
   ;; like \"~/.emacs.d/server\". It has no effect if
   ;; `dotspacemacs-enable-server' is nil.
   ;; (default nil)
   dotspacemacs-server-socket-dir "~/.cache/"

   ;; If non-nil, advise quit functions to keep server open when quitting.
   ;; (default nil)
   dotspacemacs-persistent-server nil

   ;; List of search tool executable names. Spacemacs uses the first installed
   ;; tool of the list. Supported tools are `rg', `ag', `pt', `ack' and `grep'.
   ;; (default '("rg" "ag" "pt" "ack" "grep"))
   dotspacemacs-search-tools '("rg" "ag" "pt" "ack" "grep")

   ;; Format specification for setting the frame title.
   ;; %a - the `abbreviated-file-name', or `buffer-name'
   ;; %t - `projectile-project-name'
   ;; %I - `invocation-name'
   ;; %S - `system-name'
   ;; %U - contents of $USER
   ;; %b - buffer name
   ;; %f - visited file name
   ;; %F - frame name
   ;; %s - process status
   ;; %p - percent of buffer above top of window, or Top, Bot or All
   ;; %P - percent of buffer above bottom of window, perhaps plus Top, or Bot or All
   ;; %m - mode name
   ;; %n - Narrow if appropriate
   ;; %z - mnemonics of buffer, terminal, and keyboard coding systems
   ;; %Z - like %z, but including the end-of-line format
   ;; If nil then Spacemacs uses default `frame-title-format' to avoid
   ;; performance issues, instead of calculating the frame title by
   ;; `spacemacs/title-prepare' all the time.
   ;; (default "%I@%S")
   dotspacemacs-frame-title-format "%t@%S" ; "%a" / "%f"

   ;; Format specification for setting the icon title format
   ;; (default nil - same as frame-title-format)
   dotspacemacs-icon-title-format nil

   ;; Color highlight trailing whitespace in all prog-mode and text-mode derived
   ;; modes such as c++-mode, python-mode, emacs-lisp, html-mode, rst-mode etc.
   ;; (default t)
   dotspacemacs-show-trailing-whitespace t

   ;; Delete whitespace while saving buffer. Possible values are `all'
   ;; to aggressively delete empty line and long sequences of whitespace,
   ;; `trailing' to delete only the whitespace at end of lines, `changed' to
   ;; delete only whitespace for changed lines or `nil' to disable cleanup.
   ;; (default nil)
   dotspacemacs-whitespace-cleanup nil ; manually turn on - ws-butler-mode

   ;; If non-nil activate `clean-aindent-mode' which tries to correct
   ;; virtual indentation of simple modes. This can interfere with mode specific
   ;; indent handling like has been reported for `go-mode'.
   ;; If it does deactivate it here.
   ;; (default t)
   dotspacemacs-use-clean-aindent-mode t

   ;; Accept SPC as y for prompts if non-nil. (default nil)
   dotspacemacs-use-SPC-as-y nil

   ;; If non-nil shift your number row to match the entered keyboard layout
   ;; (only in insert state). Currently supported keyboard layouts are:
   ;; `qwerty-us', `qwertz-de' and `querty-ca-fr'.
   ;; New layouts can be added in `spacemacs-editing' layer.
   ;; (default nil)
   dotspacemacs-swap-number-row nil

   ;; Either nil or a number of seconds. If non-nil zone out after the specified
   ;; number of seconds. (default nil)
   dotspacemacs-zone-out-when-idle nil

   ;; Run `spacemacs/prettify-org-buffer' when
   ;; visiting README.org files of Spacemacs. (default nil)
   dotspacemacs-pretty-docs t

   ;; If nil the home buffer shows the full path of agenda items
   ;; and todos. If non-nil only the file name is shown. (default nil)
   dotspacemacs-home-shorten-agenda-source nil

   ;; If non-nil then byte-compile some of Spacemacs files.
   dotspacemacs-byte-compile nil

   ) ; end-of setq-default

  ) ;; end-of dotspacemacs/init ()

```


### <span class="section-num">2.6</span> User Initialization {#h:cdb48d42-eb9d-4253-94b6-31f5461aeab1}

```elisp
;;; User Initialization

(defun dotspacemacs/user-init ()

;;;; User Initilization

  ;; Always prompt in minibuffer (no GUI)
  (setq use-dialog-box nil
        ;; Use y or n instead of yes or no
        use-short-answers t
        ;; Confirm before quitting
        confirm-kill-emacs 'y-or-n-p
        )

  ;; emacsclient -s ~/.cache/spacemacs-server -n
  ;; (setq server-name "spacemacs-server") ; default "server"
  (setq server-name (concat "spacemacs-server-" emacs-major-version-string))

  ;; Don’t compact font caches during GC.
  ;; 이 설정 정말 중요하다. 특히 org-superstar 사용 시 필수!
  (setq inhibit-compacting-font-caches t)

;;;; unset keys before layers are loaded

  (global-unset-key (kbd "M-c"))  ; unset capitalize-word

  (global-unset-key (kbd "<f1>"))
  (global-unset-key (kbd "<f2>"))
  (global-unset-key (kbd "<f3>"))
  (global-unset-key (kbd "<f4>"))
  (global-unset-key (kbd "<f5>"))
  (global-unset-key (kbd "<f7>"))
  (global-unset-key (kbd "<f8>"))
  (global-unset-key (kbd "<f10>"))
  (global-unset-key (kbd "<f12>"))

  ;; Disable deprecation warnings about =cl=. The =cl= library has been deprecated, but lots of packages still use it. I can't control that, but I can disable the warnings.
  (setq byte-compile-warnings '(cl-functions))

;;;; Pinned 'stable' packages

  ;; (add-to-list 'package-pinned-packages '(evil . "nongnu") t)
  (add-to-list 'package-pinned-packages '(clojure-mode . "nongnu") t)
  (add-to-list 'package-pinned-packages '(cider . "nongnu") t)
  (add-to-list 'package-pinned-packages '(async . "gnu") t)
  (add-to-list 'package-pinned-packages '(transient . "gnu") t) ; for magit

;;;; Load 'per-machine' configuration

  ;; Most of my per-environment config done via =customize= and is in .custom.el.
  ;; However, some config is more involved, such as packages I just want in one
  ;; environment and not the others.  To that end, let's load a file that can contain
  ;; those customizations.
  (let ((per-machine-filename (concat dotspacemacs-directory "per-machine.el")))
    (when (file-exists-p per-machine-filename)
      (load-file per-machine-filename)))

;;;; 'OFF' Emacspeak

  ;; emacspeak 사용. 28 안정화 버전에서만 사용
  ;; (if (< emacs-major-version 29)
  ;;  )

  (defvar *run-emacspeak* nil) ; on / off

;;;; Load custom-file

  ;; 2023-12-04 debugging for termux
  ;; (setq async-debug t)

  ;; layers/+emacs/org/packages.el
  (setq spacemacs-space-doc-modificators
        '(org-indent-mode
          view-mode
          hide-line-numbers
          alternative-emphasis
          alternative-tags-look
          link-protocol
          org-block-line-face-remap
          org-kbd-face-remap
          resize-inline-images))

  (setq spacemacs-theme-comment-bg nil
        spacemacs-theme-org-bold t
        spacemacs-theme-org-height nil
        spacemacs-theme-org-highlight nil)

  (setq custom-file (concat dotspacemacs-directory "emacs-custom.el"))
  ;; (if (file-exists-p custom-file)
  ;;    (load-file custom-file)
  ;;   (load-file (concat dotspacemacs-directory "assets/emacs-custom-default.el")))

  ;; FIXME
  (load-file (concat dotspacemacs-directory "assets/emacs-custom-default.el"))

  ;; (setq custom-file "~/.spacemacs.d/emacs-custom.el")
  ;; (load custom-file)

  ) ;; end-of init

```


### <span class="section-num">2.7</span> User Environment {#h:c5c4d33c-b99d-4e50-9ad4-e3c388aefe61}

```elisp
;;; User Environment

(defun dotspacemacs/user-env ()
  (spacemacs/load-spacemacs-env)
  )
```


### <span class="section-num">2.8</span> User Configuration {#h:50195944-e2ab-4fed-b1d8-6d2de4450134}

```elisp
;;; User Configuration

(defun dotspacemacs/user-config ()
  (setq user-configs-file (file-truename (concat dotspacemacs-directory "user-configs.el")))
  (load user-configs-file)
  (setq keybindings-config-file (file-truename (concat dotspacemacs-directory "user-keybindings.el")))
  (load keybindings-config-file)
  )
```


## <span class="section-num">3</span> Define <kbd>Custom-Layers</kbd> (`layers/<jh-xxx>`) {#h:fc1ea247-5ef6-4c4e-a807-6c7b2482af90}

Each of the following subsections is dedicated to an individual custom
library. These are "packages" of mine that are only relevant to my
Emacs configuration, even though they are designed in accordance with
best practices for packaging Emacs Lisp code. Many of my public-facing
packages for Emacs started out as custom libraries like these

Please bear in mind that the code I write here is not necessarily as
high quality as what I put in my public packages, meaning that I do
not test it as much and do not try to make it perfect.


### <span class="section-num">3.1</span> The `load-29.el` library {#h:dbdfc774-c389-403f-95e7-17fa62c53d84}

```elisp
;;; load-29.el -*- lexical-binding: t -*-

(setq-default
 ;; List of configuration layers to load.
 dotspacemacs-configuration-layers
 '(
   jh-base
   jh-completion
   jh-visual
   jh-workspace
   jh-checker
   jh-writing
   jh-reading
   jh-coding
   jh-org
   jh-misc
   )
 )

;;; load-29.el ends here
```


### <span class="section-num">3.2</span> The `jh-base` layer {#h:9a20f8cf-1907-454b-9d1f-7cbfeaae77d8}




#### <span class="section-num">3.2.1</span> The `jh-base` layer.el {#h:c54d24ae-a1ee-4d2a-879e-358fa863fe86}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   spacemacs-defaults

   (helpful :packages (not popwin))

   (shell
    :variables
    shell-default-shell 'multi-vterm ; 'vterm
    shell-default-term-shell (concat root-path "usr/bin/zsh")
    spacemacs-vterm-history-file-location "~/.zsh_history"
    shell-default-full-span nil ; default t
    shell-default-position 'bottom
    ;; shell-default-height 30
    )
   )
 )

```


#### <span class="section-num">3.2.2</span> The `jh-base` packages.el {#h:e1cf75e4-367c-44f8-b2b8-e0ae39716cff}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-base-packages
      '(
    ;;;;; spacemacs-bootstrap
        which-key
        helpful
    ;;;;; spacemacs-defaults

        xref
        abbrev
        dabbrev
        dired
        savehist

        ;; eldoc

        ;; recentf
        ;; dired-x
        ;; shell
        ;; winner
        ;; saveplace

        ;; tar-mode

        ;; archive-mode
        ;; conf-mode
        ;; cus-edit
        ;; display-line-numbers
        ;; electric-indent-mode
        ;; easypg
        ;; ediff
        ;; help-fns+
        ;; hi-lock
        ;; image-mode
        ;; imenu
        ;; occur-mode
        ;; package-menu
        ;; page-break-lines
        ;; process-menu
        ;; quickrun
        ;; subword
        ;; uniquify
        ;; url
        ;; visual-line-mode
        ;; whitespace
        ;; zone

    ;;;;; Additional 'built-in' packages
        calendar
        tramp
        proced
        man
        ;; pmc ; music player daemon
        time
        ;; time-stamp
        fortune
        goto-addr

    ;;;;; Additional packages

        emacsql-sqlite-builtin

        (term-keys :location (recipe :fetcher github :repo "junghan0611/term-keys"))

        xclip

        ;; gc-buffers ; too aggressive
        ;; (explain-pause-mode :location (recipe :fetcher github :repo "lastquestion/explain-pause-mode"))
        )
      )

    ```

<!--list-separator-->

2.  Configurations

    ```elisp

    ;;;; time-stamp

    ;; ~/sync/man/dotsamples/vanilla/kimim-dotfiles-cjk/README.org
    ;; (defun jh-base/init-time-stamp ()
    ;;   (use-package time-stamp
    ;;    :config
    ;;    (setq time-stamp-active t)
    ;;    (setq time-stamp-warn-inactive t)
    ;;    (setq time-stamp-format "%:y-%02m-%02d %3a %02H:%02M:%02S Junghanacs")
    ;;    (add-hook 'write-file-functions 'time-stamp))
    ;;   )

    ;;;; sqlite-builtin

    (defun jh-base/init-emacsql-sqlite-builtin ()
      (require 'emacsql-sqlite-builtin)
      )

    ;;;; Helpful

    ;; tshu/lisp/editor-misc.el
    (defun jh-base/post-init-helpful ()
      (setq helpful-max-buffers 3)

      (defun helpful-reuse-window (buffer-or-name)
        "Switch to helpful BUFFER-OR-NAME.

    The logic is simple, if we are currently in the helpful buffer,
    reuse it's window, otherwise create new one."
        (if (eq major-mode 'helpful-mode)
            (pop-to-buffer-same-window buffer-or-name)
          (pop-to-buffer buffer-or-name)))

      (setq helpful-switch-buffer-function #'helpful-reuse-window)

      (add-hook 'helpful-mode-hook #'visual-line-mode)
      (add-hook 'help-mode-hook #'visual-line-mode)

      ;; (with-eval-after-load 'ibuffer
      ;;   (add-to-list 'ibuffer-help-buffer-modes 'helpful-mode))
      )

    ;;;; Which-key

    (defun jh-base/post-init-which-key ()
      (setq which-key-sort-order 'which-key-key-order-alpha) ; minemacs
      (setq which-key-ellipsis "..")

      ;; (unless *is-termux*
      ;;   (setq which-key-side-window-location 'top))

      (when *is-termux*
        (setq which-key-min-display-lines 5))


      ;; (setq which-key-max-description-length 40) ; spacemacs 32

      ;; (setq which-key-idle-delay 0.4)
      ;; (setq which-key-sort-order 'which-key-key-order) ;; default
      ;; same as default, except single characters are sorted alphabetically
      ;; same as default, except all prefix keys are grouped together at the end
      ;; (setq which-key-sort-order 'which-key-prefix-then-key-order) ; spacemacs

      )

    ;;;; Dired

    ;;;;; dired

    ;; https://systemcrafters.cc/emacs-from-scratch/effortless-file-management-with-dired/

    (defun jh-base/post-init-dired ()
      ;; -al ; spacemacs
      ;; Make sure to use the long name of flags when exists
      ;; eg. use "--almost-all" instead of "-A"
      ;; Otherwise some commands won't work properly
      ;; (setq dired-listing-switches
      ;;       "-l --almost-all --human-readable --time-style=long-iso --group-directories-first --no-group")
      ;; (setq dired-listing-switches
      ;;       "-h -g -u --time-style=long-iso --group-directories-first -o")
      ;; (setq dired-listing-switches
      ;;  "-goah --group-directories-first --time-style=long-iso") ; emacs writing studio

      ;; /victoro-dotfiles-elixir/lisp/core-emacs-packages-config.el:23
      (setq
       ;; Better dired flags:
       ;; `-l' is mandatory
       ;; `-a' shows all files
       ;; `-h' uses human-readable sizes
       ;; `-F' appends file-type classifiers to file names (for better highlighting)
       ;; dired-listing-switches "-laFGh1v --group-directories-first"
       dired-ls-F-marks-symlinks t ; -F marks links with @
       ;; Inhibit prompts for simple recursive operations
       dired-recursive-copies 'always
       ;; Auto-copy to other Dired split window
       dired-dwim-target t)
      (setq dired-auto-revert-buffer t)

      (setq dired-listing-switches
            "-AGFhlv --group-directories-first --time-style=long-iso")

      (setq dired-kill-when-opening-new-dired-buffer t)
      (setq dired-make-directory-clickable t) ; Emacs 29.1
      (setq dired-free-space nil) ; Emacs 29.1
      (setq dired-mouse-drag-files t) ; Emacs 29.1
      (setq dired-guess-shell-alist-user ; those are the suggestions for ! and & in Dired
            '(("\\.\\(png\\|jpe?g\\|tiff\\)" "feh" "xdg-open")
              ("\\.\\(mp[34]\\|m4a\\|ogg\\|flac\\|webm\\|mkv\\)" "mpv" "xdg-open")
    		  (".*" "xdg-open")))

      ;; (setq dired-recursive-deletes 'always)
      (setq copy-directory-create-symlink t)
      (setq dired-hide-details-hide-symlink-targets nil) ; default t

      ;; In Emacs 29 there is a binding for `repeat-mode' which let you
      ;; repeat C-x C-j just by following it up with j.  For me, this is a
      ;; problem as j calls `dired-goto-file', which I often use.
      ;; (define-key dired-jump-map (kbd "j") nil)

      (add-hook 'dired-mode-hook 'dired-hide-details-mode)
      (add-hook 'dired-mode-hook
                (lambda ()
                  (interactive)
                  (setq-local truncate-lines t) ; Do not wrap lines
                  ;; (visual-line-mode -1)
                  (hl-line-mode 1)))

      (defun my/dired-home ()
        "Open dired at $HOME"
        (interactive)
        (dired (expand-file-name "~")))

      (defun my/dired-open-this-subdir ()
        (interactive)
        (dired (dired-current-directory)))

      (defun my/dired-kill-all-subdirs ()
        (interactive)
        (let ((dir dired-directory))
          (kill-buffer (current-buffer))
          (dired dir)))

      ;; (define-key image-mode-map (kbd "k") 'image-kill-buffer)
      ;; (define-key image-mode-map (kbd "<right>") 'image-next-file)
      ;; (define-key image-mode-map (kbd "<left>") 'image-previous-file)
      (define-key dired-mode-map (kbd "C-<return>") 'image-dired-dired-display-external)

      ;; from prot
      (define-key dired-mode-map (kbd "C-+") #'dired-create-empty-file)
      (setq dired-isearch-filenames 'dwim)
      (setq dired-create-destination-dirs 'ask) ; Emacs 27
      ;; (setq dired-vc-rename-file t)             ; Emacs 27
      ;; (setq dired-do-revert-buffer (lambda (dir) (not (file-remote-p dir)))) ; Emacs 28

      ;; (setq dired-create-destination-dirs-on-trailing-dirsep t) ; Emacs 29

      ;; wdired is a mode that allows you to rename files and directories by editing the
      ;; =dired= buffer itself.
      (require 'wdired)
      (setq wdired-allow-to-change-permissions t)
      (setq wdired-create-parent-directories t)
      (add-hook 'wdired-mode-hook 'evil-normal-state)
      (evil-define-key 'normal wdired-mode-map (kbd "^") 'evil-first-non-blank)

      ;; embark better
      ;; (defun kimim/dired-other-window ()
      ;;   (interactive)
      ;;   (let ((other-dired-buffer (dired-dwim-target-directory)))
      ;;     (if other-dired-buffer
      ;;         (dired-other-window other-dired-buffer)
      ;;       (dired-jump-other-window))))

      (defun kimim/dired-get-org-link ()
        "get a link from dired for org"
        (interactive)
        (let ((filename (dired-get-filename)))
          (kill-new (concat
                     "[["
                     (concat "~/" (file-relative-name filename "~"))
                     "]["
                     (file-name-nondirectory filename)
                     "]]"))))

      (evil-define-key 'normal dired-mode-map
        (kbd "C-c C-e") 'wdired-change-to-wdired-mode
        (kbd "C-c l") 'kimim/dired-get-org-link
        ;; (kbd "C-c C-o") 'kimim/dired-other-window ; use embark-act o
        (kbd ".") 'consult-line
        (kbd "h") 'dired-up-directory
        (kbd "l") 'dired-find-file
        (kbd "S-SPC") 'dired-toggle-marks
        ;; <normal-state> RET            dired-find-file
        ;; <normal-state> S-<return>     dired-find-file-other-window
        )
      )

    ;;;;; TODO dired-like mode for the trash (trashed.el)

    ;; (prot-emacs-package trashed
    ;;   (:install t)
    ;;   (:delay 60)
    ;;   (setq trashed-action-confirmer 'y-or-n-p)
    ;;   (setq trashed-use-header-line t)
    ;;   (setq trashed-sort-key '("Date deleted" . t))
    ;;   (setq trashed-date-format "%Y-%m-%d %H:%M:%S"))

    ;;;;; TODO image-dired

    ;; (prot-emacs-package image-dired
    ;;                     (:delay 60)
    ;;                     (setq image-dired-thumbnail-storage 'standard)
    ;;                     (setq image-dired-external-viewer "xdg-open")
    ;;                     (setq image-dired-thumb-size 80)
    ;;                     (setq image-dired-thumb-margin 2)
    ;;                     (setq image-dired-thumb-relief 0)
    ;;                     (setq image-dired-thumbs-per-row 4)
    ;;                     (define-key image-dired-thumbnail-mode-map
    ;;                                 (kbd "<return>") #'image-dired-thumbnail-display-external))
    ;;;; Savehist

    (defun jh-base/pre-init-savehist ()
      (spacemacs|use-package-add-hook savehist
        :post-init
        ;; 기본이 100, 스페이스맥스 1000
        (setq history-delete-duplicates t) ; default nil
        (setq history-length 500)

        (add-to-list 'savehist-additional-variables 'corfu-history)
        ;; (corfu-history evil-jumps-history projectile-project-command-history mark-ring global-mark-ring search-ring regexp-search-ring extended-command-history kill-ring)
        )
      )

    ;;;; Dabbrev : Dynamic Word Completion

    (defun jh-base/init-dabbrev ()
      (use-package dabbrev
        :init
        ;; (setq dabbrev-abbrev-char-regexp "\\sw\\|\\s_") ; prot
        (setq dabbrev-abbrev-char-regexp "[A-Za-z-_]") ; tshu
        (setq dabbrev-ignored-buffer-regexps '("\\.\\(?:pdf\\|jpe?g\\|png\\)\\'"))
        (setq dabbrev-abbrev-skip-leading-regexp "[$*/=~']")

        ;; (setq dabbrev-upcase-means-case-search t) ; default nil
        (setq dabbrev-check-all-buffers nil) ;; default t
        :config
        (let ((map global-map))
          (define-key map (kbd "M-/") #'dabbrev-expand)
          (define-key map (kbd "C-M-/") #'dabbrev-completion)))
      )

    ;;;; Abbrev : Abbreviations

    (defun jh-base/post-init-abbrev ()
      ;; (setq abbrev-file-name (concat org-directory "/var/abbrev_defs"))
      (setq abbrev-file-name "~/sync/org/var/abbrev_defs")

      (read-abbrev-file abbrev-file-name)
      (setq save-abbrevs t)
      (setq-default abbrev-mode t)
      )

    ;;;; Tramp

    (defun jh-base/init-tramp ()
      (use-package tramp
        :defer 8
        :init
        ;; :commands tramp-file-local-name
        ;; Set default connection mode to SSH
        (setq tramp-default-method "ssh")

        (setq remote-file-name-inhibit-cache 60 ; default 10
              tramp-verbose 1 ; default 3
              vc-handled-backends '(SVN Git))

        :config
        (add-to-list 'tramp-remote-path 'tramp-own-remote-path)
        (defun my/show-server-edit-buffer (buffer)
          ;; TODO: Set a transient keymap to close with 'C-c C-c'
          (split-window-vertically -15)
          (other-window 1)
          (set-buffer buffer))
        ;; (setq server-window #'my/show-server-edit-buffer)
        )
      )

    ;;;; Man

    (defun jh-base/init-man ()
      (use-package man
        :defer 10
        :after evil
        :config
        (setq Man-notify-method 'pushy) ; does not obey `display-buffer-alist'
        (let ((map Man-mode-map))
          (define-key map (kbd "i") #'Man-goto-section)
          (define-key map (kbd "g") #'Man-update-manpage))

        (evil-define-key '(motion normal visual) Man-mode-map
          "M-n" 'Man-next-section
          "M-p" 'Man-previous-section
          "]]" 'Man-next-section
          "[[" 'Man-previous-section
          "gs" 'Man-goto-section
          ">"  'Man-follow-manual-reference)
        )
      )

    ;;;; Calendar

    (defun jh-base/init-calendar ()
      (use-package calendar
        :config
        ;; (setq org-agenda-start-on-weekday nil)
        (add-hook 'calendar-today-visible-hook 'calendar-mark-today)
        (setq calendar-date-style 'iso ;; YYYY/MM/DD
              calendar-mark-holidays-flag t
              calendar-week-start-day 1 ;; 0 Sunday, 1 Monday
              calendar-mark-diary-entries-flag nil
              calendar-latitude user-calendar-latitude
              calendar-longitude user-calendar-longitude
              calendar-location-name user-calendar-location-name
              calendar-time-display-form
              '(24-hours ":" minutes
                         (if time-zone " (") time-zone (if time-zone ")")))
        )
      )

    ;;;; Proced

    (defun jh-base/init-proced ()
      (use-package proced
        :defer 10
        :init
        (setq proced-auto-update-flag t)
        (setq proced-enable-color-flag t) ; Emacs 29
        (setq proced-auto-update-interval 5)
        (setq proced-descend t)
        (setq proced-filter 'user))
      )

    ;;;; Time-format and world-clock

    (defun jh-base/init-time ()
      (use-package time
        :after calendar
        :init
        ;; (setq display-time-format " |🅆%U📅%Y-%m-%d⏲%H:%M| ")
        ;; (setq display-time-format " |%m/%d|%H:%M| ")
        (setq display-time-format " | %a %e %b, %H:%M | ")
        ;; Covered by `display-time-format'
        ;; (setq display-time-24hr-format t)
        ;; (setq display-time-day-and-date t)
        (setq display-time-interval 30) ; default 60
        (setq display-time-default-load-average nil)

        ;; NOTE 2022-09-21: For all those, I have implemented my own solution
        ;; that also shows the number of new items, although it depends on
        ;; notmuch: the `notmuch-indicator' package.
        (setq display-time-mail-directory nil)
        (setq display-time-mail-function nil)
        (setq display-time-use-mail-icon nil)
        (setq display-time-mail-string nil)
        (setq display-time-mail-face nil)

        ;; World clock
        (setq zoneinfo-style-world-list
              '(("America/Los_Angeles" "Los Angeles")
                ("America/Chicago" "Chicago")
                ("Brazil/Acre" "Rio Branco")
                ("America/New_York" "New York")
                ("Brazil/East" "Brasília")
                ("Europe/Lisbon" "Lisbon")
                ("Europe/Brussels" "Brussels")
                ("Europe/Athens" "Athens")
                ("Asia/Tbilisi" "Tbilisi")
                ("Asia/Yekaterinburg" "Yekaterinburg")
                ("Asia/Shanghai" "Shanghai")
                ("Asia/Seoul" "Seoul")
                ("Asia/Vladivostok" "Vladivostok")))

        ;; All of the following variables are for Emacs 28
        (setq world-clock-list t)
        (setq world-clock-time-format "%R %z  %A %d %B")
        (setq world-clock-buffer-name "*world-clock*") ; Placement handled by `display-buffer-alist'
        (setq world-clock-timer-enable t)
        (setq world-clock-timer-second 60)
        )
      )

    ;;;; xref

    (defun jh-base/post-init-xref ()
      ;; (setq xref-file-name-display 'project-relative)
      (setq xref-search-program 'ripgrep)

      (setq xref-show-xrefs-function #'consult-xref)
      ;; Use completing-read interface instead of definitions buffer (needs xref 1.1.0)
      ;; (setq xref-show-definitions-function #'xref-show-definitions-completing-read)
      (setq xref-show-definitions-function #'consult-xref) ;; default xref-show-definitions-buffer
      )

    ;;;; fortune

    ;; not work on termux
    (defun jh-base/init-fortune ()
      (use-package fortune
        :if (not (or my/remote-server *is-termux*))
        :init
        (setq fortune-always-compile nil)
        (setq fortune-dir (concat root-path "usr/share/games/fortunes/advice"))
        (setq fortune-file (concat root-path "usr/share/games/fortunes/advice"))
        ))

    ;;;; Unless 'window-system'

    ;;;;; term-keys

    (defun jh-base/init-term-keys ()
      (use-package term-keys
        :unless window-system
        :demand
        :config
        (term-keys-mode t)
        ))

    ;;;;; xclip

    (defun jh-base/init-xclip ()
      (use-package xclip
        :if (not (or my/remote-server *is-termux*))
        :config
        (unless (display-graphic-p)
          (xclip-mode 1))
        ))

    ;;;; Actionable URL

    ;; Actionable URLs in Emacs buffers via [[http://xenodium.com/#actionable-urls-in-emacs-buffers][Álvaro Ramírez]].

    (defun jh-base/init-goto-addr ()
      (use-package goto-addr
        ;; :hook ((compilation-mode . goto-address-mode)
        ;;        (prog-mode . goto-address-prog-mode)
        ;;        (eshell-mode . goto-address-mode)
        ;;        (shell-mode . goto-address-mode))
        :bind (:map goto-address-highlight-keymap
                    ("C-c C-o" . goto-address-at-point))
        :commands (goto-address-prog-mode
                   goto-address-mode)
        :config
        (global-goto-address-mode +1)
        )
      )

    ;;;; eldoc

    ;; Helps with rendering documentation
    ;; https://www.masteringemacs.org/article/seamlessly-merge-multiple-documentation-sources-eldoc
    ;; (setq x-gtk-resize-child-frames 'resize-mode) ;; needed
    ;; (setq eldoc-idle-delay 0.3) ; default 0.5

    ;; (defun jh-base/post-init-eldoc ()
    ;;   (use-package eldoc
    ;;     :init (setq
    ;;                 ;; eldoc-idle-delay 0.3 ; 0.5
    ;;                 eldoc-echo-area-display-truncation-message t
    ;;                 eldoc-echo-area-use-multiline-p nil
    ;;                 eldoc-echo-area-prefer-doc-buffer t
    ;;                 ;; eldoc-display-functions '(eldoc-display-in-echo-area eldoc-display-in-buffer)
    ;;                 ;; eldoc-minor-mode-string nil
    ;;                 )
    ;;     (setq eldoc-documentation-strategy 'eldoc-documentation-compose)
    ;;     ;; :config
    ;;     ;; (add-to-list 'display-buffer-alist
    ;;     ;;              '("^\\*eldoc for" display-buffer-at-bottom
    ;;     ;;                (window-height . 4)))
    ;;     ;; (global-eldoc-mode +1)
    ;;     )
    ;;   )

    ```


#### <span class="section-num">3.2.3</span> The `jh-base` funcs.el {#h:cf47df94-23ce-4b7d-8f01-bdf808daef99}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:


;;; Code:

;;;; Helper Functions

(defun td/bind-keys (conses &optional mode-map)
  "Bind several keybinds using a list of `CONSES'.
  Binds will be global unless the optional `MODE-MAP' is specified."
  (dolist (combo conses)
    (if (or (consp mode-map) (keymapp mode-map))
        (define-key mode-map (kbd (car combo)) (cdr combo))
      (if mode-map (warn "Optional %s `MODE-MAP' was invalid: %s" (type-of mode-map) mode-map))
      (global-set-key (kbd (car combo)) (cdr combo)))))

(defun td/add-hooks (modes func)
  "Set several hooks from a list of `CONSES'.
  Adds '-hook' onto the end of the symbols for brevity."
  (dolist (mode modes)
    (add-hook (intern (concat (symbol-name mode) "-hook")) func)))

(defun td/auto-mode (modes)
  "Add the `MODES' to the `auto-mode-alist'."
  (dolist (mode modes)
    (add-to-list 'auto-mode-alist mode)))

(defun td/filter-nil (seq)
  "Filter out nil items from sequence `SEQ'."
  (seq-filter #'(lambda (item) item) seq))

(defun td/is-file-buffer (buffer)
  "Test if a buffer belongs to a file on the system. Returns non-nil if it does."
  (let ((file (buffer-file-name buffer)))
    (when file
      (file-exists-p file))))

;;;; Extra Functions

(defun describe-last-function ()
  (interactive)
  (describe-function last-command))

(defun expose (function &rest args)
  "Return an interactive version of FUNCTION, 'exposing' it to the user."
  (lambda ()
    (interactive)
    (apply function args)))

(defun what-face (pos)
  "Show the name of face under point."
  (interactive "d")
  (let ((face (or (get-char-property (point) 'read-face-name)
                  (get-char-property (point) 'face))))
    (if face (message "Face: %s" face) (message "No face at %d" pos))))


;; /injae-dotfiles/module/+util.el
;; text random
(defun randomize-region (beg end)
  (interactive "r")
  (if (> beg end)
      (let (mid) (setq mid end end beg beg mid)))
  (save-excursion
    ;; put beg at the start of a line and end and the end of one --
    ;; the largest possible region which fits this criteria
    (goto-char beg)
    (or (bolp) (forward-line 1))
    (setq beg (point))
    (goto-char end)
    ;; the test for bolp is for those times when end is on an empty
    ;; line; it is probably not the case that the line should be
    ;; included in the reversal; it isn't difficult to add it
    ;; afterward.
    (or (and (eolp) (not (bolp)))
        (progn (forward-line -1) (end-of-line)))
    (setq end (point-marker))
    (let ((strs (shuffle-list
                 (split-string (buffer-substring-no-properties beg end)
                               "\n"))))
      (delete-region beg end)
      (dolist (str strs)
        (insert (concat str "\n"))))))

(defun shuffle-list (list)
  "Randomly permute the elements of LIST.
  All permutations equally likely."
  (let ((i 0) j temp
        (len (length list)))
    (while (< i len)
      (setq j (+ i (random (- len i))))
      (setq temp (nth i list))
      (setcar (nthcdr i list) (nth j list))
      (setcar (nthcdr j list) temp)
      (setq i (1+ i))))
  list)

(defun new-buffer-save (name buffer-major-mode)
  (interactive)
  (let ((buffer (generate-new-buffer name)))
    (switch-to-buffer buffer)
    (set-buffer-major-mode buffer)
    (funcall buffer-major-mode)
    (setq buffer-offer-save t))
  )

(defun new-buffer (name buffer-major-mode)
  (let ((buffer (generate-new-buffer name)))
    (switch-to-buffer buffer)
    (set-buffer-major-mode buffer)
    (funcall buffer-major-mode))
  )

(defun new-no-name-buffer ()
  (interactive)
  (new-buffer "untitled" 'text-mode)
  )

;; (defun numcores ()
;;   "Return the number of logical processors on this system."
;;   (or
;;    ;; Linux
;;    (when (file-exists-p "/proc/cpuinfo")
;;      (with-temp-buffer
;;        (insert-file-contents "/proc/cpuinfo")
;;        (how-many "^processor[[:space:]]+:")))
;;    ;; Windows
;;    (let ((number-of-processors (getenv "NUMBER_OF_PROCESSORS")))
;;      (when number-of-processors
;;        (string-to-number number-of-processors)))
;;    ;; BSD+OSX
;;    (with-temp-buffer
;;      (ignore-errors
;;        (when (zerop (call-process "sysctl" nil t nil "-n" "hw.ncpu"))
;;          (string-to-number (buffer-string)))))
;;    ;; Default
;;    1))

  ;;; measure-time
;; (defmacro measure-time (&rest body)
;;   "Measure and return the running time of the code block."
;;   (declare (indent defun))
;;   ;; Fresh garbage collection before making any measurements.
;;   (garbage-collect)
;;   (let ((start (make-symbol "start")))
;;     `(let ((,start (float-time)))
;;        ,@body
;;        (- (float-time) ,start))))

  ;;; random
;; (defun insert-random (n)
;;   "Insert a random number between 0 and the prefix argument."
;;   (interactive "P")
;;   (insert (number-to-string (random n))))
;; ;; (global-set-key (kbd "C-c r") 'insert-random)

;; (cl-defun insert-random-hex (&optional (size 64))
;;   "Insert a random, SIZE-bit number as hexadecimal."
;;   (interactive)
;;   (let ((string (make-string (/ size 4) 0))
;;         (digits "0123456789abcdef"))
;;     (dotimes (i (/ size 4))
;;       (setf (aref string i) (aref digits (cl-random 16))))
;;     (insert string)))

;; (defun eval-and-replace (value)
;;   "Evaluate the sexp at point and replace it with its value."
;;   (interactive (list (eval-last-sexp nil)))
;;   (kill-sexp -1)
;;   (insert (format "%S" value)))

(defun lookup-quote (word)
  (interactive (list (thing-at-point 'word)))
  (browse-url (format "http://en.wikiquote.org/wiki/%s" word)))

  ;;; Dictionary lookup
(defun lookup-word (word)
  (interactive (list (thing-at-point 'word)))
  (browse-url (format "http://en.wiktionary.org/wiki/%s" word)))
;; (global-set-key (kbd "M-#") 'lookup-word)

  ;;; Quick switch to scratch buffers
;; (defmacro scratch-key (key buffer-name mode)
;;   `(global-set-key ,key (lambda ()
;;                           (interactive)
;;                           (switch-to-buffer ,buffer-name)
;;                           (unless (eq major-mode ',mode)
;;                             (,mode)))))

;; (declare-function js2-mode nil)
;; (declare-function clojure-mode nil)
;; (scratch-key (kbd "C-c s") "*scratch*"    emacs-lisp-mode)
;; (scratch-key (kbd "C-c j") "*javascript*" js2-mode)
;; (scratch-key (kbd "C-c x") "*css*"        css-mode)
;; (scratch-key (kbd "C-c h") "*html*"       html-mode)

;; ID: 72dc0a9e-c41c-31f8-c8f5-d9db8482de1e
;; (defun find-all-files (dir)
;;   "Open all files and sub-directories below the given directory."
;;   (interactive "DBase directory: ")
;;   (let* ((list (directory-files dir t "^[^.]"))
;;          (files (cl-remove-if 'file-directory-p list))
;;          (dirs (cl-remove-if-not 'file-directory-p list)))
;;     (dolist (file files)
;;       (find-file-noselect file))
;;     (dolist (dir dirs)
;;       (find-file-noselect dir)
;;       (find-all-files dir))))

;; Process menu killing
;; (define-key process-menu-mode-map (kbd "k") #'process-menu-kill)
;; (defun process-menu-kill ()
;;   "Kill selected process in the process menu buffer."
;;   (interactive)
;;   (let ((process (get-text-property (point) 'tabulated-list-id)))
;;     (when (processp process) (delete-process process))
;;     (run-at-time 0.1 nil (lambda ()
;;                            (let ((n (line-number-at-pos)))
;;                              (revert-buffer)
;;                              (forward-line (1- n)))))))

;; Help mode assistance
;; (defun push-first-button ()
;;   "Find and push the first button in this buffer, intended for `help-mode'."
;;   (interactive)
;;   (cl-block :find-button
;;     (goto-char (point-min))
;;     (while (< (point) (point-max))
;;       (if (get-text-property (point) 'button)
;;           (cl-return-from :find-button (push-button))
;;         (forward-char)))))

;; Tabs
;; (defun toggle-tab-width ()
;;   (interactive)
;;   (let* ((loop [8 4 2])
;;          (match (or (cl-position tab-width loop) -1)))
;;     (setf tab-width (aref loop (mod (1+ match) (length loop))))))
;; (global-set-key (kbd "C-h t") #'toggle-tab-width)

;;;; Open user-files

(defun my/open-fortune-quotes ()
  (interactive )
  (find-file "~/sync/org/fortunes/"))
(defun my/open-dotsamples-readme ()
  (interactive )
  (find-file "~/sync/man/dotsamples/README.org"))
(defun my/open-org-workflow-path ()
  (interactive )
  (find-file "~/sync/org/roam/workflow/"))
;; (defun my/open-literate-org ()
;;   (interactive)
;;   (find-file "~/sync/org/roam/configs"))
(defun my/open-tempel-templates ()
  (interactive)
  (find-file (concat dotspacemacs-directory "tempel-templates.eld")))
(defun my/open-hunspell-personal ()
  (interactive)
  (find-file"~/.hunspell_personal"))
(defun my/open-elfeed-list ()
  (interactive)
  (find-file "~/sync/org/elfeed/elfeed.org"))
(defun my/open-dict-ko-mydata ()
  (interactive)
  (find-file "~/sync/org/dict-ko-mydata.yaml"))
(defun my/open-abbrev-defs ()
  (interactive)
  (find-file "~/sync/org/var/abbrev_defs"))
(defun my/open-refile ()
  (interactive)
  (find-file org-refile-file))
(defun my/open-mobile ()
  (interactive)
  (find-file org-mobile-file))
(defun my/open-quote ()
  (interactive)
  (find-file org-quote-file))
(defun my/open-iam ()
  (interactive)
  (find-file org-iam-file))
(defun my/open-links ()
  (interactive)
  (find-file org-links-file))
(defun my/open-tags ()
  (interactive)
  (find-file org-tags-file))
(defun my/open-now ()
  (interactive)
  (find-file org-now-file))
(defun my/open-org-project  ()
  (interactive)
  (find-file org-projectile-file))

;;;; now day clear-kill-ring

(defun ag/region-or-word-at-point-str ()
  "Returns string of selected region or word at point"
  (let* ((bds (if (use-region-p)
                  (cons (region-beginning) (region-end))
                (bounds-of-thing-at-point 'word)))
         (p1 (car bds))
         (p2 (cdr bds)))
    (buffer-substring-no-properties p1 p2)))

(defun clear-kill-ring ()
  "Clear the results on the kill ring."
  (interactive) (setq kill-ring nil))

(defun now () (interactive)
       (insert (shell-command-to-string "date")))

(defun day ()
  "Insert string for today's date nicely formatted in American style,
    e.g. Sunday, September 17, 2000."
  (interactive)                 ; permit invocation in minibuffer
  (insert (format-time-string "%A, %B %e, %Y")))

(defun today ()
  "Insert string for today's date nicely formatted in American style,
    e.g. 2000-10-12."
  (interactive)                 ; permit invocation in minibuffer
  (insert (format-time-string "%Y-%m-%d")))

(defun today-pretty ()
  "Insert string for today's date nicely formatted in American style,
    e.g. 2000-10-12."
  (interactive)                 ; permit invocation in minibuffer
  (insert (format-time-string "%A, %b %d, %Y")))

(defun toyear ()
  "Insert string for today's date nicely formatted in American style,
    e.g. 2000."
  (interactive)                 ; permit invocation in minibuffer
  (insert (format-time-string "%Y")))

;;;; formatting files at once

;; https://stackoverflow.com/questions/2551632/how-to-format-all-files-under-a-dir-in-emacs

;; Next, open a Dired buffer at the top level of the directory under which you
;; want to change all of the files. Give the dired command a numeric prefix so
;; that it will ask for the switches to give to the ls command, and add the R
;; (recurse) switch: C-u C-x d R RET your-directory RET. 그런 다음, 모든 파일을
;; 변경하려는 디렉터리의 최상위 수준에서 Dired 버퍼를 엽니다. dired 명령에 숫자
;; 접두사를 지정하여 ls 명령에 전달할 스위치를 요청하고 R 을 추가합니다. (재귀)
;; 스위치를 추가합니다: C-u C-x d R RET your-directory RET .

;; Next, mark all of the regular files in the recursive directory listing: first
;; * / to mark all the directories, then * t to toggle the selection. 그런 다음
;; 재귀적 디렉터리 목록에 있는 모든 일반 파일을 표시합니다. 먼저 * / 을 눌러
;; 모든 디렉터리를 표시한 다음 * t 을 눌러 선택 항목을 전환합니다.

;; Finally, run the above command: M-x indent-marked-files.

;; Be aware that if you already have any buffers visiting any of the target
;; files, they'll be killed by indent-marked-files. Also be aware that none of
;; the file changes will be undoable; use with caution! I tested it in a simple
;; case and it seems to work as described, but I make no guarantees. 대상 파일을
;; 방문하는 버퍼가 이미 있는 경우 indent-marked-files 에 의해 버퍼가 종료된다는
;; 점에 유의하세요. . 또한 어떤 파일 변경도 되돌릴 수 없으므로 주의해서
;; 사용하세요! 간단한 케이스에서 테스트해 본 결과 설명대로 작동하는 것 같지만,
;; 보장할 수는 없습니다.

(defun my/indent-marked-files ()
  (interactive)
  (dolist (file (dired-get-marked-files))
    (find-file file)
    (indent-region (point-min) (point-max))
    (save-buffer)
    (kill-buffer nil)))

;; Usage :: '~/.spacemacs.d/layers/' and then '.el'
;; https://stackoverflow.com/questions/2551632/how-to-format-all-files-under-a-dir-in-emacs
(defun my/indent-files (directory extension)
  (interactive (list (read-directory-name "Directory: ")
                     (read-string "File extension: ")))
  (dolist (file (directory-files-recursively directory extension))
    (find-file file)
    (indent-region (point-min) (point-max))
    (save-buffer)
    (kill-buffer nil)))

```


#### <span class="section-num">3.2.4</span> The `jh-base` keybindings.el {#h:22d5f7b1-2705-4c52-a4c2-3b81ca858507}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

(spacemacs/set-leader-keys "od" 'my/dired-home)

(spacemacs/declare-prefix-for-mode 'dired-mode "ms" "subdir")
(spacemacs/set-leader-keys-for-major-mode 'dired-mode
  "ss" 'dired-maybe-insert-subdir
  "ss" 'dired-maybe-insert-subdir
  "sl" 'dired-maybe-insert-subdir
  "sq" 'dired-kill-subdir
  "sk" 'dired-prev-subdir
  "sj" 'dired-next-subdir
  "sS" 'my/dired-open-this-subdir
  "sQ" 'my/dired-kill-all-subdirs
  )

(spacemacs/set-leader-keys-for-major-mode 'dired-mode
  "h" 'dired-hide-details-mode
  "o" 'dired-omit-mode)

;;; extran functions

(spacemacs/declare-prefix "oe"  "extra functions★")
(spacemacs/set-leader-keys
  "oel" 'describe-last-function
  "oew" 'what-face
  ;; TODO add custom functions here
  )

(spacemacs/set-leader-keys "ok" 'clear-kill-ring)

;;; global-map

(global-set-key (kbd "M-Y") 'clear-kill-ring)
```


### <span class="section-num">3.3</span> The `jh-completion` layer {#h:26c906b9-4447-401f-84d5-d8e0139ec65c}




#### <span class="section-num">3.3.1</span> The `jh-completion` layer.el {#h:d0db1976-c6df-44d4-a59e-94612dea91f2}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   ;; auto-yasnippet yasnippet yasnippet-snippets
   (auto-completion :packages (hippie-exp smartparens yasnippet))

   ;; 2023-10-05 back to company
   ;; Add tool tips to show doc string of functions
   ;; Show snippets in the auto-completion popup
   ;; Show suggestions by most commonly used
   ;; (auto-completion
   ;;  :packages (not auto-complete ac-ispell)
   ;;  :variables
   ;;  auto-completion-enable-snippets-in-popup t
   ;;  auto-completion-enable-sort-by-usage t
   ;;  auto-completion-idle-delay 0.1
   ;;  auto-completion-minimum-prefix-length 2 ; default 2
   ;;  ;; auto-completion-complete-with-key-sequence "fd"
   ;;  auto-completion-enable-help-tooltip nil
   ;;  auto-completion-use-company-posframe nil
   ;;  spacemacs-default-company-backends
   ;;  '((company-dabbrev-code company-keywords)
   ;;    company-files company-dabbrev)
   ;;  )

   (compleseus :variables
               compleseus-engine 'vertico
               marginalia-align 'left
               )
   )
 )

```


#### <span class="section-num">3.3.2</span> The `jh-completion` packages.el {#h:2aee6ef1-4127-4a22-b850-a14d54e85204}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-completion-packages
      '(
        embark
        consult
        tempel
        tempel-collection

        corfu
        cape

        popon
        (corfu-terminal :location (recipe :fetcher codeberg :repo "akib/emacs-corfu-terminal"))

        ;; consult-eglot ; need project.el

        vertico
        ;; vertico-posframe
        ;; consult-dir ; need project.el
        ;; (tabnine-capf :location (recipe :fetcher github
        ;;                                 :repo "50ways2sayhard/tabnine-capf"
        ;;                                 :files ("*.el" "*.sh")))
        )
      )

    ```

<!--list-separator-->

2.  Configurations

    ```elisp

    ;;;; vertico with 'top'

    (defun jh-completion/post-init-vertico ()

      ;; 2023-12-02
      (define-key vertico-map (kbd"C-<return>") 'vertico-quick-exit)
      ;; vertico-directory
      (define-key vertico-map (kbd "RET")   'vertico-directory-enter)
      (define-key vertico-map (kbd "DEL")   'vertico-directory-delete-char)
      (define-key vertico-map (kbd "M-DEL") 'vertico-directory-delete-word)

      ;; 2023-05-23 org-roam-node-find
      (define-key vertico-map (kbd "C-n") #'spacemacs/next-candidate-preview)
      (define-key vertico-map (kbd "C-p") #'spacemacs/previous-candidate-preview)

      (unless *is-termux*
        (require 'vertico-buffer)
        (setq vertico-resize 'grow-only)
        ;; (setq vertico-count 10)
        (setq vertico-buffer-display-action `(display-buffer-in-side-window
                                              (window-height . ,(+ 3 vertico-count))
                                              (side . top)))
        (vertico-mode +1)
        (vertico-buffer-mode +1)
        )

      (when *is-termux*
        (setq vertico-resize nil)
        (setq vertico-count 7))
      )

    ;;;;; DONT vertico-posframe

    ;; (defun jh-completion/init-vertico-posframe ()
    ;;   (use-package vertico-posframe
    ;;     :if window-system
    ;;     :ensure t
    ;;     :custom
    ;;     (vertico-posframe-parameters
    ;;      '((left-fringe . 10)
    ;;        (right-fringe . 10))))
    ;;   )

    ;;;; Embark

    ;; /karthink-dotfiles-popper/lisp/setup-embark.el
    (defun jh-completion/post-init-embark ()
      (require 'embark)

      (defun my/split-window-right (&optional size)
        "Split the selected window into two windows, one above the other.
    The selected window is below.  The newly split-off window is
    below and displays the same buffer.  Return the new window."
        (interactive "P")
        (select-window
         (if size
             (split-window (frame-root-window)
                           (floor (frame-width) 2)
                           t nil)
           (split-window-right size)))
        (when (interactive-p)
          (if (featurep 'consult)
              (consult-buffer)
            (call-interactively #'switch-to-buffer))))

      (defun my/split-window-below (&optional size)
        "Split the selected window into two side-by-side windows.
    The selected window is on the left.  The newly split-off window
    is on the right and displays the same buffer.  Return the new
    window."
        (interactive "P")
        (select-window
         (if size
             (split-window (frame-root-window)
                           (floor (frame-height) 2)
                           nil nil)
           (split-window-below size)))
        (when (interactive-p)
          (if (featurep 'consult)
              (consult-buffer)
            (call-interactively #'switch-to-buffer))))

      ;; Segfaults ?!
      ;; (defun my/find-file-dir (file)
      ;;   (interactive (list (read-file-name "Jump to dir of file: ")))
      ;;   (dired (file-name-directory file)))

      ;; Extra embark actions
      (eval-when-compile
        (defmacro my/embark-ace-action (fn)
          `(defun ,(intern (concat "my/embark-ace-" (symbol-name fn))) ()
             (interactive)
             (with-demoted-errors "%s"
               (require 'ace-window)
               (aw-switch-to-window (aw-select nil))
               (call-interactively (symbol-function ',fn)))))

        (defmacro my/embark-split-action (fn split-type)
          `(defun ,(intern (concat "my/embark-"
                                   (symbol-name fn)
                                   "-"
                                   (car (last  (split-string
                                                (symbol-name split-type) "-"))))) ()
             (interactive)
             (funcall #',split-type)
             (call-interactively #',fn))))

      ;; (define-key embark-file-map (kbd "o") (my/embark-ace-action find-file))
      ;; (define-key embark-file-map (kbd "j") #'my/find-file-dir)
      (define-key embark-file-map (kbd "2") (my/embark-split-action find-file my/split-window-below))
      (define-key embark-file-map (kbd "3") (my/embark-split-action find-file my/split-window-right))
      (define-key embark-file-map (kbd "4") #'find-file-other-window)
      (define-key embark-file-map (kbd "5") #'find-file-other-frame)

      ;; (define-key embark-buffer-map (kbd "o") (my/embark-ace-action switch-to-buffer))
      (define-key embark-buffer-map (kbd "2") (my/embark-split-action switch-to-buffer my/split-window-below))
      (define-key embark-buffer-map (kbd "3") (my/embark-split-action switch-to-buffer my/split-window-right))

      ;; (define-key embark-bookmark-map (kbd "o") (my/embark-ace-action bookmark-jump))
      (define-key embark-bookmark-map (kbd "2") (my/embark-split-action bookmark-jump my/split-window-below))
      (define-key embark-bookmark-map (kbd "3") (my/embark-split-action bookmark-jump my/split-window-right))

      (define-key embark-library-map (kbd "2") (my/embark-split-action find-library my/split-window-below))
      (define-key embark-library-map (kbd "3") (my/embark-split-action find-library my/split-window-right))
      ;; (define-key embark-library-map (kbd "o") (my/embark-ace-action find-library))

      ;; By default, embark doesn't know how to handle org-links.  Let's provide a way.
      ;; https://github.com/ahyatt/emacs-setup/commit/15fcb33eadbd14ef3d2f2a5b464aeaf830ba0c45
      (defun ash/org-link ()
        "Get the link from an org-link."
        (require 's)
        (when (eq major-mode 'org-mode)
          (let ((context (org-element-context)))
            (cond ((and (eq (car context) 'link)
                        (equal (plist-get (cadr context) :type) "file"))
                   (cons 'file (plist-get (cadr context) :path)))
                  ((and (eq (car context) 'link)
                        (member (plist-get (cadr context) :type) '("http" "https")))
                   (cons 'url (concat (plist-get (cadr context) :type) ":" (s-trim-right (plist-get (cadr context) :path)))))
                  (t nil)))))
      (add-to-list 'embark-target-finders 'ash/org-link)
      )

    ;;;; Tempel

    ;; Template-based in-buffer completion (tempel.el)
    ;; NOTE 2023-01-19: Check the `templates'
    (defun jh-completion/init-tempel ()
      (use-package tempel
        :ensure t
        :bind (("M-+" . tempel-complete) ;; Alternative tempel-expand
               ("M-*" . tempel-insert))
        :config
        ;; (setq tempel-trigger-prefix "<") ; conflits with evil-shift
        (setq tempel-path (expand-file-name "tempel-templates.eld" dotspacemacs-directory))
        ;; Use concrete keys because of org mode
        ;; "M-RET" #'tempel-done
        ;; "M-{" #'tempel-previous
        ;; "M-}" #'tempel-next
        ;; "M-<up>" #'tempel-previous
        ;; "M-<down>" #'tempel-next

        ;; 2023-10-19 disable my custom
        (define-key tempel-map (kbd "RET") #'tempel-done)
        (define-key tempel-map (kbd "M-n") #'tempel-next)
        (define-key tempel-map (kbd "M-p") #'tempel-previous)
        )
      )

    (defun jh-completion/init-tempel-collection ()
      (use-package tempel-collection
        :defer t
        :after tempel
        ))

    ;;;; Consult
    ;;;;; post consult

    ;; consult-grep-args
    ;; consult-ripgrep-args

    (defun jh-completion/pre-init-consult ()
      (spacemacs|use-package-add-hook consult
        :post-config
        (consult-customize
         consult-theme
         :preview-key '("M-." "C-SPC"
                        :debounce 3.0 any) ; 2 seconds

         ;; For `consult-line' change the prompt and specify multiple preview
         ;; keybindings. Note that you should bind <S-up> and <S-down> in the
         ;; `minibuffer-local-completion-map' or `vertico-map' to the commands which
         ;; select the previous or next candidate.
         consult-buffer
         consult-ripgrep
         consult-git-grep
         consult-grep
         consult-bookmark
         consult-yank-pop ; needed

         ;; ADD Belows
         consult-line ; :prompt "Consult-line: "
         consult-recent-file
         consult-xref
         consult-org-heading
         consult-outline ; 2023-05-23
         eli/consult-org-roam-heading

         spacemacs/consult-line
         my/custom-switch-to-buffer

         spacemacs/compleseus-switch-to-buffer
         spacemacs/compleseus-search-dir
         spacemacs/compleseus-search-auto
         spacemacs/compleseus-find-file ; 2023-05-14 추가
         spacemacs/embark-preview ; 2023-05-23
         spacemacs/compleseus-search-default
         :preview-key '("M-." "C-SPC"
                        ;; :debounce 0.3 "C-M-j" "C-M-k" ; conflict puni
                        :debounce 0.3 "<up>" "<down>" "C-n" "C-p"
                        ))
        )
      )

    ;;;;; DONT consult-eglot

    ;; (defun jh-completion/init-consult-eglot ()
    ;;   (use-package consult-eglot
    ;;     :after consult eglot
    ;;     :config
    ;;     ;; (spacemacs/set-leader-keys-for-major-mode m
    ;;     ;;   "cc" 'consult-eglot-symbols)
    ;;     ;; Provide `consult-lsp' functionality from `consult-eglot', useful
    ;;     ;; for packages which relay on `consult-lsp' (like `dirvish-subtree').
    ;;     ;; (defalias 'consult-lsp-file-symbols #'consult-eglot-symbols)
    ;;     (define-key eglot-mode-map (kbd "C-c e c") #'consult-eglot-symbols)
    ;;     )
    ;;   )

    ;;;; DONT corfu and cape

    (defun jh-completion/init-popon ()
      (use-package popon)
      )

    (defun jh-completion/init-corfu-terminal ()
      (use-package corfu-terminal
        :after popon
        :config
        (setq corfu-terminal-position-right-margin 1)
        (unless (display-graphic-p)
          (corfu-terminal-mode +1)
          )
        ))

    ;;;;; corfu

    ;; C-n/p 는 corfu-next/corfu-previous
    ;; C-j - newline-and-indent 가 편하다. completion 무시하고 바로 뉴 라인
    ;; tab/backtab 은 jump-out-of-pair,

    (defun jh-completion/init-corfu ()
      (use-package corfu
        ;; TAB-and-Go customizations
        :demand t
        :ensure t
        :custom
        (corfu-cycle t)           ;; Enable cycling for `corfu-next/previous'
        (corfu-auto t)
        (corfu-auto-prefix 2)
        (corfu-bar-width 0.5)
        (corfu-auto-delay 0.3)
        (corfu-on-exact-match nil)
        (corfu-preselect nil)
        (corfu-min-width 35)
        (corfu-max-width 80)
        ;; Use TAB for cycling, default is `corfu-complete'.
        :bind
        ;; see my init.el
        (:map corfu-map ("M-." . corfu-move-to-minibuffer))
        :init
        (setq completion-cycle-threshold 3
              tab-always-indent t)
        (use-package corfu-echo
          :hook (corfu-mode . corfu-echo-mode))
        (use-package corfu-history
          :hook (corfu-mode . corfu-history-mode))

        ;; (with-eval-after-load 'eldoc
        ;;   (eldoc-add-command #'corfu-insert))
        (global-corfu-mode)
        )
      )

    ;;;;; CAPE : Completion At Point Extensions

    ;; (defun jh-completion/init-cape-yasnippet ()
    ;;   (use-package cape-yasnippet :ensure))

    (defun jh-completion/init-cape ()
      (use-package cape
        :ensure t
        :after corfu
        :demand t
        :init
        ;; /gopar-dotfiles-youtuber/README.org:1371
        (setq cape-dabbrev-min-length 2)
        (setq cape-dabbrev-check-other-buffers 'some)
        (defun corfu-enable-in-minibuffer ()
          "Enable Corfu in the minibuffer if `completion-at-point' is bound."
          (when (where-is-internal #'completion-at-point (list (current-local-map)))
            (setq-local corfu-auto nil) ;; Enable/disable auto completion
            ;; Disable automatic echo and popup
            (setq-local corfu-echo-delay nil)
            (corfu-mode 1)))
        (add-hook 'minibuffer-setup-hook #'corfu-enable-in-minibuffer)
        ;; Bind dedicated completion commands
        ;; Alternative prefix keys: C-c p, M-p, M-+, ...
        :bind (("M-p p" . completion-at-point) ;; capf
               ("M-p t" . complete-tag)        ;; etags
               ("M-p d" . cape-dabbrev)        ;; or dabbrev-completion
               ("M-p h" . cape-history)
               ("M-p f" . cape-file)
               ("M-p k" . cape-keyword)
               ("M-p s" . cape-elisp-symbol)
               ("M-p e" . cape-elisp-block)
               ("M-p a" . cape-abbrev)
               ("M-p l" . cape-line)
               ("M-p w" . cape-dict)
               ("M-p :" . cape-emoji)
               ;; ("M-c p \\" . cape-tex)
               ;; ("M-c p _" . cape-tex)
               ;; ("M-c p ^" . cape-tex)
               ;; ("M-c p &" . cape-sgml)
               ;; ("M-c p r" . cape-rfc1345)
               )
        )
      )

    ;;     (define-prefix-command 'my-cape-map)
    ;;     (define-key global-map (kbd "M-c") 'my-cape-map)
    ;;     (let ((map my-cape-map))
    ;;       ;; (define-key map (kbd "a") 'cape-abbrev)
    ;;       (define-key map (kbd "d") 'cape-dict)
    ;;       (define-key map (kbd "d") 'cape-dabbrev)
    ;;       (define-key map (kbd "e") 'hippie-expand)
    ;;       (define-key map (kbd "p") 'completion-at-point)
    ;;       (define-key map (kbd "t") 'complete-tag)
    ;;       (define-key map (kbd "h") 'cape-history)
    ;;       ;; (define-key map (kbd "y") 'cape-yasnippet)
    ;;       (define-key map (kbd "f") 'cape-file)
    ;;       (define-key map (kbd "k") 'cape-keyword)
    ;;       (define-key map (kbd "s") 'cape-symbol)
    ;;       ;; (define-key map (kbd "l") 'cape-line)
    ;;       ;; (define-key map (kbd "r") 'cape-rfc1345)
    ;;       ;; (define-key map (kbd "\\") 'cape-tex)
    ;;       )
    ;;     ;; https://github.com/minad/cape#company-adapter
    ;;     )
    ;;   )

    ;;; packages.el ends here
    ```


#### <span class="section-num">3.3.3</span> The `jh-completion` funcs.el {#h:2cbd3103-62de-44ee-882a-9217e21b7223}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;Pre-select nearest heading for consult-org-heading and consult-outline using
;; (defvar consult--previous-point nil
;;   "Location of point before entering minibuffer.
;; Used to preselect nearest headings and imenu items.")

;; (defun consult--set-previous-point ()
;;   "Save location of point. Used before entering the minibuffer."
;;   (setq consult--previous-point (point)))

;; (defun consult-vertico--update-choose (&rest _)
;;   "Pick the nearest candidate rather than the first after updating candidates."
;;   (when (and consult--previous-point
;;              (memq current-minibuffer-command
;;                    '(consult-org-heading consult-outline)))
;;     (setq vertico--index
;;           (max 0 ; if none above, choose the first below
;;                (1- (or (seq-position
;;                         vertico--candidates
;;                         consult--previous-point
;;                         (lambda (cand point-pos) ; counts on candidate list being sorted
;;                           (> (cl-case current-minibuffer-command
;;                                (consult-outline
;;                                 (car (consult--get-location cand)))
;;                                (consult-org-heading
;;                                 (get-text-property 0 'consult--candidate cand)))
;;                              point-pos)))
;;                        (length vertico--candidates))))))
;;   (setq consult--previous-point nil))

;;; eli customs

;;;###autoload
(defun eli/consult-buffer()
  (interactive)
  (consult-buffer)
  (when (member (prefix-numeric-value current-prefix-arg) '(4 16 64))
    (delete-other-windows)))

;;;###autoload
(defun eli/consult-git-grep ()
  "Search with git grep for files in current git dir. "
  (interactive)
  (consult-git-grep (vc-root-dir)))

;;;###autoload
(defun eli/consult-ripgrep-single-file (file-path)
  "Search single file use `consult-ripgrep'."
  (interactive)
  (let ((consult-project-function (lambda (_x) nil))
        (consult-ripgrep-args
         (concat "rg "
                 "--null "
                 "--line-buffered "
                 "--color=never "
                 "--line-number "
                 "--smart-case "
                 "--no-heading "
                 "--max-columns=1000 "
                 "--max-columns-preview "
                 "--with-filename "
                 (shell-quote-argument file-path))))
    (consult-ripgrep)))

;;;###autoload
(defun eli/consult-git-ripgrep (dir)
  "Search single file use `consult-ripgrep'."
  (interactive
   (list (read-directory-name "Select directory: " "~/.emacs.d/site-lisp/")))
  (let ((consult-project-function (lambda (_x) nil))
        (consult-ripgrep-args (string-replace "." "-g *.el ."
                                              consult-ripgrep-args)))
    (consult-ripgrep dir)))

;;;###autoload
(defun my/info-search-elisp-emacs ()
  "Search info through `consult-info'."
  (interactive)
  (consult-info "elisp" "emacs"))

;;;###autoload
(defun eli/consult-org-roam-heading (&optional match)
  "Jump to an Org-roam heading.

MATCH is as in org-map-entries and determine which
entries are offered."
  (interactive)
  (consult-org-heading match '(directory-files-recursively org-roam-directory "\\.org")))

;;; my/custom-swith-to-buffer

;;;###autoload
(defun my/custom-switch-to-buffer ()
  "`consult-buffer' with buffers provided by persp."
  (interactive)
  (consult-buffer
   '(consult--source-hidden-buffer ; 'SPC'
     consult--source-persp-buffers ; l layout
     consult--source-modified-buffers ; M
     consult--source-buffer ; b -- ADD
     consult--source-recent-file ; f
     consult--source-bookmark ; m

     ;; consult--source-file-register ; r -- ADD
     ;; org-roam-buffer-source ; k -- ADD
     ;; consult-projectile--source-projectile-buffer ; j -- ADD
     )))

;;; gopar's custom

(defun gopar/consult-line (&optional arg)
  "Start consult search with selected region if any.
If used with a prefix, it will search all buffers as well."
  (interactive "p")
  (let ((cmd (if current-prefix-arg '(lambda (arg) (consult-line-multi t arg)) 'consult-line)))
    (if (use-region-p)
        (let ((regionp (buffer-substring-no-properties (region-beginning) (region-end))))
          (deactivate-mark)
          (funcall cmd regionp))
      (funcall cmd ""))))

;;;###autoload
(defun consult-line-symbol ()
  (interactive)
  (consult-line
   (if (region-active-p)
       (buffer-substring-no-properties
        (region-beginning) (region-end))
     (thing-at-point 'symbol t))))

```


#### <span class="section-num">3.3.4</span> The `jh-completion` config.el {#h:2598ea32-754f-4b15-9f60-a012e152c48b}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; consult--source

;; consult-narrow-key : "<" ; spacemacs default
;; Search C-h f for more "bookmark jump" handlers.
;; (setq consult-bookmark-narrow
;;       `((?d "Docview" ,#'doc-view-bookmark-jump)
;;         (?e "Eshell" ,#'eshell-bookmark-jump)
;;         (?f "File" ,#'bookmark-default-handler)
;;         (?h "Help" ,#'help-bookmark-jump)
;;         (?i "Info" ,#'Info-bookmark-jump)
;;         (?m "Man" ,#'Man-bookmark-jump)
;;         ;; (?p "PDF" ,#'pdf-view-bookmark-jump)
;;         (?v "VC Dir" ,#'vc-dir-bookmark-jump)
;;         (?w "EWW" ,#'prot-eww-bookmark-jump)))
```


#### <span class="section-num">3.3.5</span> The `jh-completion` keybindings.el {#h:5b6a2706-112b-4125-97ea-08111336255c}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; (setq-default completion-in-region-function #'consult-completion-in-region)

(global-set-key (kbd "M-s M-i") 'my/info-search-elisp-emacs) ; bugfix
(global-set-key (kbd "M-s m") 'consult-line-multi) ; bugfix
(global-set-key (kbd "M-g t") 'consult-minor-mode-menu)

(spacemacs/set-leader-keys "bb" 'consult-buffer)
(spacemacs/set-leader-keys "bB" 'my/custom-switch-to-buffer)
(spacemacs/set-leader-keys "sS" 'consult-line-symbol)
(spacemacs/set-leader-keys "s M-s" 'gopar/consult-line)

(global-set-key (kbd "M-s b") 'consult-buffer)
(global-set-key (kbd "M-s B") 'my/custom-switch-to-buffer)

(unless (display-graphic-p) ; terminal
  (define-key vertico-map (kbd "M-<return>") #'vertico-exit-input)
  )
```


### <span class="section-num">3.4</span> The `jh-visual` layer {#h:cf78dad4-8419-490b-be25-32886d861bd8}




#### <span class="section-num">3.4.1</span> The `jh-visual` layer.el {#h:ca3058cb-ef6b-4a05-a90c-7d2b8ebf35fd}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   (colors :packages (color-identifiers-mode
                      rainbow-mode))

   ;; ligatures on text-mode may cause issues with org-mode and magit
   (unicode-fonts
    :packages (ligature) ; not unicode-fonts
    :variables
    unicode-fonts-enable-ligatures nil
    ;; unicode-fonts-ligature-set '("<==" "<~~" "==>" "~~>"
    ;;                              "<=>" "<==>" "->" "-->" "<-" "<--"
    ;;                              "<->" "<-->")
    ;; unicode-fonts-ligature-modes '(prog-mode)
    )

   imenu-list

   (spacemacs-modeline
    :packages (anzu spaceline))
   ;; :packages (doom-modeline))

   (spacemacs-visual
    :packages (ansi-colors desktop display-fill-column-indicator popwin
                           posframe zoom-frm all-the-icons)) ; delete popup
   ))

```


#### <span class="section-num">3.4.2</span> The `jh-visual` packages.el {#h:dd471480-6c60-483f-af91-a0cacb179b12}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:


    ;;;; Package Lists

    (defconst jh-visual-packages
      '(
        hl-todo
        popwin

        ;; doom-modeline
        spaceline
        minions

        imenu-list
        ;; evil

        ;; 새로 등록하는 패키지
        ct ; color tool
        auto-dim-other-buffers

        list-unicode-display
        celestial-mode-line
        hammy
        hide-mode-line
        modus-themes
        ef-themes
        doom-themes
        nerd-icons
        ;; nerd-icons-dired
        nerd-icons-completion

        popper
        fontaine
        popup
        kind-icon

        dashboard

    ;;;;; DEPRECATED  pkgs
        ;; (indent-bars :location (recipe :fetcher github :repo "jdtsmith/indent-bars"))
        ;; dimmer
        ;; highlight-indent-guides
        ;; doom-themes
        )
      )
    ```

<!--list-separator-->

2.  Configurations

    ```elisp
    ;;;; nerd-icons

    (defun jh-visual/init-nerd-icons ()
      (use-package nerd-icons :demand t :ensure t))

    ;; (defun jh-visual/init-nerd-icons-dired ()
    ;;   (use-package nerd-icons-dired
    ;;     :if window-system
    ;;     :after nerd-icons
    ;;     ;; :hook (dired-mode . nerd-icons-dired-mode)
    ;;     ))

    (defun jh-visual/init-nerd-icons-completion ()
      (use-package nerd-icons-completion
        :if window-system
        :after (marginalia nerd-icons)
        :config
        (nerd-icons-completion-mode))
      )

    ;;;; Tools

    ;;;;; ct color
    (defun jh-visual/init-ct ()
      (use-package ct :ensure t))

    ;;;;; auto-dim-other-buffers
    (defun jh-visual/init-auto-dim-other-buffers ()
      (use-package auto-dim-other-buffers
        :ensure t
        :if (display-graphic-p)
        :config
        (auto-dim-other-buffers-mode t)
        )
      )

    ;;;;; imenu-list

    (defun jh-visual/post-init-imenu-list ()
      (setq imenu-list-focus-after-activation nil
            imenu-list-auto-resize nil)
      (setq imenu-list-position 'left)
      (setq imenu-list-size 45) ; default 0.3
      (setq imenu-list-idle-update-delay 1.0) ; default 0.5
      ;; (setq-default imenu-list-mode-line-format nil)
      ;; (remove-hook 'imenu-list-major-mode-hook #'imenu-list--set-mode-line)
      (add-hook 'imenu-list-major-mode-hook #'spacemacs/toggle-truncate-lines-on)
      )

    ;;;; Modeline


    ;;;;; Minions
    (defun jh-visual/init-minions ()
      (use-package minions
        :demand t
        :config
        (setq minions-mode-line-lighter "Ⓜ")
        (defun jh-visual/enable-mode-line-addons ()
          (minions-mode 1))
        (add-hook 'spacemacs-post-user-config-hook #'jh-visual/enable-mode-line-addons)
        )
      )

    ;;;;; Spaceline

    (defun jh-visual/pre-init-spaceline ()
      (spacemacs|use-package-add-hook spaceline-config
        :pre-config
        (setq display-time-default-load-average nil)
        (setq spaceline-global-p nil) ; remove global-mode-string
        (setq spaceline-show-default-input-method nil) ; default nil

        (spaceline-toggle-major-mode-off)
        ;; (spaceline-toggle-buffer-size-off)
        ;; (spaceline-toggle-minor-modes-off)
        ;; (spaceline-toggle-window-number-off)
        ;; (spaceline-toggle-purpose-off)
        ;; (spaceline-toggle-buffer-encoding-abbrev-off)

        :post-config
        ;; Change to spaceline's default
        (setq spaceline-highlight-face-func 'spaceline-highlight-face-evil-state)
        ;; (set-face-attribute 'spaceline-evil-emacs nil :background "#bd93f9" :foreground "#000000")

        ;; 2023-06-21 Fix buffer-id for file-path
        ;; (setq spaceline-buffer-id-max-length 35) ; default 45
        (spaceline-define-segment buffer-id
          (spaceline--string-trim-from-center
           (if (buffer-file-name)
               (abbreviate-file-name (buffer-file-name))
             (s-trim (powerline-buffer-id (if active 'mode-line-buffer-id 'mode-line-buffer-id-inactive))))
           spaceline-buffer-id-max-length))

        (require 'minions)

        ;; NOTE: This will be expanded whenever I find a mode that should not
        ;; be hidden
        (setq minions-prominent-modes
              (list 'defining-kbd-macro
                    'flymake-mode
                    'flycheck-mode
                    ))

        (spaceline-define-segment minor-modes
          (if (bound-and-true-p minions-mode)
              (format-mode-line minions-mode-line-modes)
            (spaceline-minor-modes-default)))
        )
      )

    ;;;;; DONT vanilla

    ;; ;; Vanilla Mode line
    ;; (setq mode-line-percent-position '(-3 "%p"))
    ;; (setq mode-line-position-column-line-format '(" %l,%c")) ; Emacs 28
    ;; (setq mode-line-defining-kbd-macro
    ;;       (propertize " Macro" 'face 'mode-line-emphasis))

    ;; ;; Thanks to Daniel Mendler for this!  It removes the square brackets
    ;; ;; that denote recursive edits in the modeline.  I do not need them
    ;; ;; because I am using Daniel's `recursion-indicator':
    ;; ;; <https://github.com/minad/recursion-indicator>.
    ;; ;; (setq-default mode-line-modes
    ;; ;;               (seq-filter (lambda (s)
    ;; ;;                             (not (and (stringp s)
    ;; ;;                                       (string-match-p
    ;; ;;                                        "^\\(%\\[\\|%\\]\\)$" s))))
    ;; ;;                           mode-line-modes))

    ;; (setq mode-line-compact nil)            ; Emacs 28

    ;; (setq-default mode-line-format
    ;;               '("%e"
    ;;                 mode-line-front-space
    ;;                 mode-line-mule-info
    ;;                 mode-line-client
    ;;                 mode-line-modified
    ;;                 mode-line-remote
    ;;                 mode-line-frame-identification
    ;;                 mode-line-buffer-identification

    ;;                 "  "
    ;;                 mode-line-position
    ;;                 mode-line-modes
    ;;                 purpose--modeline-string ; 23/01/10--07:10 :: ON
    ;;                 "  "
    ;;                 (vc-mode vc-mode)
    ;;                 "  "
    ;;                 ;; Org-mode-line-string
    ;;                 mode-line-misc-info
    ;;                 mode-line-end-spaces))
    ;; ;; (add-hook 'after-init-hook #'column-number-mode)

    ;;;;; DONT doom-modeline

    ;; (defun jh-visual/post-init-doom-modeline ()
    ;;   (use-package doom-modeline
    ;;     :ensure t
    ;;     :config
    ;;     (setq doom-modeline-time nil)
    ;;     (setq doom-modeline-time-icon nil)
    ;;     (setq doom-modeline-minor-modes nil)
    ;;     (setq doom-modeline-battery nil)
    ;;     (setq doom-modeline-bar-width 10) ; = fringe-mode
    ;;     (setq Info-breadcrumbs-in-mode-line-mode nil)

    ;;     ;; (setq doom-modeline-height 35)
    ;;     ;; (setq doom-modeline-window-width-limit (- fill-column 10))

    ;;     (setq doom-modeline-icon nil)
    ;;     (setq doom-modeline-enable-word-count nil)

    ;;     (setq doom-modeline-repl t)
    ;;     (setq doom-modeline-lsp t)
    ;;     (setq doom-modeline-github t)
    ;;     (setq doom-modeline-indent-info t)
    ;;     (setq doom-modeline-hud t)
    ;;     ;; (setq doom-modeline-window-width-limit nil)

    ;;     ;; (setq doom-modeline-buffer-file-name-style 'truncate-upto-project)
    ;;     (setq doom-modeline-buffer-file-name-style 'truncate-upto-root)
    ;;     ;; (setq doom-modeline-support-imenu t)

    ;;     (remove-hook 'display-time-mode-hook #'doom-modeline-override-time)
    ;;     (remove-hook 'doom-modeline-mode-hook #'doom-modeline-override-time)
    ;;     (doom-modeline-mode +1)
    ;;     )
    ;;   )

    ;;;;; celestial-mode-line

    (defun jh-visual/init-celestial-mode-line ()
      (use-package celestial-mode-line
        :after time
        :init
        (setq celestial-mode-line-update-interval 3600) ; default 60
        (setq celestial-mode-line-sunrise-sunset-alist '((sunrise . "🌅") (sunset . "🌄")))
        (setq celestial-mode-line-phase-representation-alist
              '((0 . "🌚")(1 . "🌛")(2 . "🌝")(3 . "🌜")))
        :config
        (celestial-mode-line-start-timer)
        )
      )

    ;;;;; Hide-mode-line

    (defun jh-visual/init-hide-mode-line ()
      (use-package hide-mode-line :defer 10))

    ;;;; Themes and Colors

    ;;;;; doom-thtmes

    (defun jh-visual/init-doom-themes ()
      (use-package doom-themes
        :ensure t
        :config
        ;; Global settings (defaults)
        (setq doom-themes-enable-bold t    ; if nil, bold is universally disabled
              doom-themes-enable-italic nil) ; if nil, italics is universally disabled
        ;; Enable flashing mode-line on errors
        (doom-themes-visual-bell-config)

        ;; Enable custom neotree theme (all-the-icons must be installed!)
        (setq doom-themes-neotree-line-spacing 1
              doom-themes-neotree-project-size 1.0
              doom-themes-neotree-folder-size 1.0)
        (doom-themes-neotree-config)
        (setq doom-themes-neotree-enable-variable-pitch t)

        ;; or for treemacs users
        (setq doom-themes-treemacs-theme "doom-colors") ; use "doom-colors" for less minimal icon theme
        (doom-themes-treemacs-config)
        ;; Corrects (and improves) org-mode's native fontification.
        (doom-themes-org-config))
      )

    ;;;;; ef-themes

    (defun jh-visual/init-ef-themes ()
      (use-package ef-themes
        :init
        (defun ef-themes-load-random-light ()
          (interactive) (ef-themes-load-random 'light))
        (defun ef-themes-load-random-dark ()
          (interactive) (ef-themes-load-random 'dark))
        :config
        (setq ef-themes-to-toggle '(ef-maris-light ef-maris-dark))

        ;; Read the doc string or manual for this one.  The symbols can be
        ;; combined in any order.
        (setq ef-themes-region '(intense no-extend neutral))

        (when (display-graphic-p) ; gui
          (setq ef-themes-variable-pitch-ui nil)

          (setq ef-themes-headings
                '(
                  (0                . (variable-pitch bold 1.2))
                  (1                . (variable-pitch bold 1.1))
                  (2                . (variable-pitch semibold 1.05))
                  (3                . (variable-pitch semibold 1.0))
                  (4                . (variable-pitch medium 1.0))
                  (5                . (variable-pitch medium 1.0))
                  (6                . (variable-pitch medium 1.0))
                  (7                . (variable-pitch medium 1.0))
                  (8                . (variable-pitch medium 1.0))
                  (agenda-date      . (variable-pitch bold 1.2))
                  (agenda-structure . (variable-pitch bold 1.1))
                  (t                . (variable-pitch medium 1.0))))
          )
        ) ; end ef-themes
      )

    ;;;;;; ef-themes backup

    ;;   ;; (defun my-ef-themes-fixed-pitch-colors ()
    ;;   ;;   (ef-themes-with-colors
    ;;   ;;     (custom-set-faces
    ;;   ;;      ;; `(org-property-value ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-metadata-value)))
    ;;   ;;      ;; `(org-drawer ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-metadata)))
    ;;   ;;      ;; `(org-tag ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-tag)))

    ;;   ;;      ;; `(org-document-info ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-metadata-value)))
    ;;   ;;      ;; `(org-document-info-keyword ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-metadata)))
    ;;   ;;      ;; `(org-meta-line ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-metadata)))

    ;;   ;;      ;; `(org-block ((,c :inherit ef-themes-fixed-pitch :background ,bg-inactive :extend t)))
    ;;   ;;      ;; `(org-block-begin-line ((,c :inherit ef-themes-fixed-pitch :background ,bg-alt :extend t)))
    ;;   ;;      ;; `(org-block-end-line ((,c :inherit org-block-begin-line)))

    ;;   ;;      ;; `(org-date ((,c :inherit ef-themes-fixed-pitch :foreground ,date-common)))
    ;;   ;;      ;; `(org-date-selected ((,c :inherit ef-themes-fixed-pitch :foreground ,date-common :inverse-video t)))
    ;;   ;;      ;; `(org-table ((,c :inherit ef-themes-fixed-pitch :foreground ,prose-table)))
    ;;   ;;      ;; `(org-formula ((,c :inherit ef-themes-fixed-pitch :foreground ,fnname)))
    ;;   ;;      ;; `(org-hide ((,c :inherit ef-themes-fixed-pitch :foreground ,bg-main)))
    ;;   ;;      )
    ;;   ;;     )
    ;;   ;;   )

    ;;   ;; (defun my-ef-themes-mode-line ()
    ;;   ;;   "Tweak the style of the mode lines."
    ;;   ;;   (ef-themes-with-colors
    ;;   ;;     (custom-set-faces
    ;;   ;;      `(fringe ((,c :background ,bg-dim)))
    ;;   ;;      `(tab-bar ((,c :inherit ef-themes-ui-variable-pitch :background ,bg-tab-bar :weight semibold)))
    ;;   ;;      `(tab-line ((,c :inherit ef-themes-ui-variable-pitch :background ,bg-tab-bar :weight semibold))) ; :height 1.0

    ;;   ;;      `(translate-paragraph-highlight-face ((,c :extend t :background ,bg-red-subtle)))

    ;;   ;;      `(jinx-misspelled ((,c :underline (:style wave :color ,magenta-cooler))))

    ;;   ;;      `(treemacs-root-face ((,c :inherit org-level-2 :underline nil :weight bold :height 1.0)))
    ;;   ;;      `(treemacs-directory-face ((,c :inherit org-level-3 :height 1.0)))
    ;;   ;;      `(treemacs-file-face ((,c :inherit org-level-4 :weight regular :height 1.0)))

    ;;   ;;      `(imenu-list-entry-face-0 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-1)))
    ;;   ;;      `(imenu-list-entry-subalist-face-0 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-1 :underline nil)))
    ;;   ;;      `(imenu-list-entry-face-1 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-2)))
    ;;   ;;      `(imenu-list-entry-subalist-face-1 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-2 :underline nil)))
    ;;   ;;      `(imenu-list-entry-face-2 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-3 )))
    ;;   ;;      `(imenu-list-entry-subalist-face-2 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-3 :underline nil)))
    ;;   ;;      `(imenu-list-entry-face-3 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-4 )))
    ;;   ;;      `(imenu-list-entry-subalist-face-3 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-4 :underline nil)))

    ;;   ;;      `(lsp-ui-doc-background ((,c (:background ,bg-dim))))
    ;;   ;;      `(lsp-ui-doc-header ((,c (:foreground ,fg-main :background ,bg-active :height 1.0))))
    ;;   ;;      `(lsp-ui-doc-url ((,c (:foreground ,fg-alt))))
    ;;   ;;      ;; `(lsp-ui-sideline-code-action ((,c (:foreground ,fg-mark-select))))

    ;;   ;;      `(highlight-indentation-current-column-face ((,c :background ,fg-alt))) ; fg-dim rainbow-0

    ;;   ;;      `(doom-modeline-input-method ((,c :weight regular :foreground ,bg-main :background ,red-cooler)))
    ;;   ;;      `(doom-modeline-evil-insert-state ((,c :weight regular :foreground ,bg-main :background ,green-cooler)))
    ;;   ;;      `(doom-modeline-evil-visual-state ((,c :weight regular :foreground ,bg-main :background ,yellow-cooler)))
    ;;   ;;      `(doom-modeline-evil-motion-state ((,c :weight regular :foreground ,bg-main :background ,blue-cooler)))
    ;;   ;;      `(doom-modeline-evil-emacs-state ((,c :weight regular :foreground ,bg-main :background ,cyan-cooler)))
    ;;   ;;      `(doom-modeline-evil-replace-state ((,c :weight regular :foreground ,bg-main :background ,magenta-cooler)))
    ;;   ;;      ;; `(doom-modeline-evil-normal-state (( )))
    ;;   ;;      ;; `(doom-modeline-evil-operator-state ((,c :inherit bold)))

    ;;   ;;      ;; org-mode
    ;;   ;;      `(org-level-2 ((,c :inherit ef-themes-heading-2 :underline t :extend t)))
    ;;   ;;      `(org-mode-line-clock ((,c :inherit bold :foreground ,modeline-info)))
    ;;   ;;      `(org-mode-line-clock-overrun ((,c :inherit bold :foreground ,modeline-err)))

    ;;   ;;      )))
    ;;   ;; (add-hook 'ef-themes-post-load-hook
    ;;   ;;           (lambda ()
    ;;   ;;             (my-ef-themes-mode-line)
    ;;   ;;             ;;            (my-ef-themes-fixed-pitch-colors)
    ;;   ;;             ))

    ;;   ;; (defun my/enable-ef-themes-variable-pitch ()
    ;;   ;;   (interactive)
    ;;   ;;   (variable-pitch-mode) ;; 모든 글꼴 변환
    ;;   ;;   (my-ef-themes-fixed-pitch-colors) ; update
    ;;   ;;   )
    ;;   )

    ;; (unless (display-graphic-p) ; terminal
    ;;   (defun my-ef-themes-colors-terminal ()
    ;;     (ef-themes-with-colors
    ;;       (custom-set-faces
    ;;        `(jinx-misspelled ((,c :underline (:style line :color ,underline-warning) :foreground ,underline-warning)))
    ;;        )))
    ;;   (add-hook 'ef-themes-post-load-hook #'my-ef-themes-colors-terminal))

    ;; (defun my-ef-themes-hl-todo-faces ()
    ;;     "Configure `hl-todo-keyword-faces' with Ef themes colors.
    ;; The exact color values are taken from the active Ef theme."
    ;;     (ef-themes-with-colors
    ;;       (setq hl-todo-keyword-faces
    ;;             `(("HOLD" . ,yellow)
    ;;               ("TODO" . ,red)
    ;;               ("NEXT" . ,blue)
    ;;               ("THEM" . ,magenta)
    ;;               ("PROG" . ,cyan-warmer)
    ;;               ("OKAY" . ,green-warmer)
    ;;               ("DONT" . ,yellow-warmer)
    ;;               ("FAIL" . ,red-warmer)
    ;;               ("BUG" . ,red-warmer)
    ;;               ("DONE" . ,green)
    ;;               ("NOTE" . ,blue-warmer)
    ;;               ("KLUDGE" . ,cyan)
    ;;               ("HACK" . ,cyan)
    ;;               ("TEMP" . ,red)
    ;;               ("FIXME" . ,red-warmer)
    ;;               ("XXX+" . ,red-warmer)
    ;;               ("REVIEW" . ,red)
    ;;               ("DEPRECATED" . ,yellow)))))
    ;; (add-hook 'ef-themes-post-load-hook #'my-ef-themes-hl-todo-faces)

    ;;;;; modus-themes

    ;; TODO 다시 테스트 해보고 정말 필요하면 다른 방법 찾아라.
    ;; (defun jh-visual/init-vterm ()
    ;;   (use-package vterm :ensure t) ; 미리 로딩 된 상태야 face 를 설정할 수 있다.
    ;;   )

    (defun jh-visual/init-modus-themes ()
      (use-package modus-themes
        :demand t
        :init
        (setq modus-themes-to-toggle (let ((hr (nth 2 (decode-time))))
                                       (if (or (< hr 7) (< 19 hr))           ; between 8 PM and 7 AM
                                           '(modus-vivendi-tinted modus-operandi-tinted) ; load dark theme first
                                         '(modus-operandi-tinted modus-vivendi-tinted))))
        :config
        (require 'modus-themes)

        (when (display-graphic-p) ; gui
          (setq modus-themes-variable-pitch-ui nil)
          ;; The `modus-themes-headings' is an alist: read the manual's
          ;; node about it or its doc string. Basically, it supports
          ;; per-level configurations for the optional use of
          ;; `variable-pitch' typography, a height value as a multiple of
          ;; the base font size (e.g. 1.5), and a `WEIGHT'.
          (setq modus-themes-headings
                '(
                  (0                . (variable-pitch bold 1.2))
                  (1                . (variable-pitch bold 1.1))
                  (2                . (variable-pitch semibold 1.05))
                  (3                . (variable-pitch semibold 1.0))
                  (4                . (variable-pitch medium 1.0))
                  (5                . (variable-pitch medium 1.0))
                  (6                . (variable-pitch medium 1.0))
                  (7                . (variable-pitch medium 1.0))
                  (agenda-date      . (variable-pitch bold 1.2))
                  (agenda-structure . (variable-pitch bold 1.1))
                  (t                . (variable-pitch medium 1.0))))
          )

        (setq modus-themes-italic-constructs nil
              modus-themes-org-blocks 'gray-background
              modus-themes-bold-constructs t
              modus-themes-custom-auto-reload t
              modus-themes-disable-other-themes t ; default t

              ;; Options for `modus-themes-prompts' are either nil (the
              ;; default), or a list of properties that may include any of those
              ;; symbols: `italic', `WEIGHT'
              ;; modus-themes-prompts '(bold)

              ;; The `modus-themes-completions' is an alist that reads two
              ;; keys: `matches', `selection'.  Each accepts a nil value (or
              ;; empty list) or a list of properties that can include any of
              ;; the following (for WEIGHT read further below):
              ;; `matches'   :: `underline', `italic', `WEIGHT'
              ;; `selection' :: `underline', `italic', `WEIGHT'
              ;; modus-themes-completions
              ;; '((matches   . (semibold))
              ;;   (selection . (semibold text-also)))
              )
        (setq modus-themes-common-palette-overrides
              `(
                ;; Customize the mode-line colors
                (fg-mode-line-active fg-main) ; Black
                ;; (bg-mode-line-active bg-blue-intense)

                ;; "Make the mode line borderless"
                (border-mode-line-active unspecified)
                (border-mode-line-inactive unspecified)
                ))
        ) ; end-of use-package
      ) ; end-of defun

    ;;;;;; modus-themes backup

    ;; 'M-x' modus-themes-preview-colors-current
    ;; (setq modus-themes-common-palette-overrides
    ;;       `(
    ;;         ;; Customize the mode-line colors
    ;;         (fg-mode-line-active fg-main) ; Black
    ;;         ;; (bg-mode-line-active bg-blue-intense)
    ;;         ;; (fg-mode-line-active unspecified)
    ;;         ;; (bg-mode-line-active unspecified)

    ;; ;; "Make the mode line borderless"
    ;; (border-mode-line-active unspecified)
    ;; (border-mode-line-inactive unspecified)

    ;;         ;; "Make matching parenthesis more or less intense"
    ;;         ;; (bg-paren-match bg-magenta-intense)
    ;;         ;; (underline-paren-match unspecified)

    ;;         ;; Links
    ;;         ;; (underline-link border)
    ;;         ;; (underline-link-visited border)
    ;;         ;; (underline-link-symbolic border)

    ;;         ;; Comments are yellow, strings are green
    ;;         (comment yellow-cooler)
    ;;         (string green-warmer)

    ;;         ;; Intense magenta background combined with the main foreground
    ;;         ;; (bg-region bg-magenta-subtle)
    ;;         ;; (fg-region fg-main)

    ;;         ;; (bg-heading-0 bg-green-nuanced) ; green
    ;;         ;; (bg-heading-1 bg-dim) ; white
    ;;         ;; (bg-heading-2 bg-yellow-nuanced) ; yellow
    ;;         ;; (bg-heading-3 bg-blue-nuanced) ; blue
    ;;         ;; (bg-heading-4 bg-magenta-nuanced) ; magenta
    ;;         ;; (bg-heading-5 bg-cyan-nuanced) ; cyan

    ;;         ;; And expand the preset here. Note that the ,@ works because we use
    ;;         ;; the backtick for this list, instead of a straight quote.
    ;;         ;; 현재 설정에 faint, intense 컬러 세트를 덮어쓰고 싶다면
    ;;         ;; ,@modus-themes-preset-overrides-faint
    ;;         ;; ,@modus-themes-preset-overrides-intense
    ;;         )
    ;;       )

    ;; (when (display-graphic-p) ; gui
    ;;   ;; (defun my-modus-themes-fixed-pitch-colors ()
    ;;   ;;   (modus-themes-with-colors
    ;;   ;;     (custom-set-faces

    ;;   ;;      ;; `(org-property-value ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-metadata-value)))
    ;;   ;;      ;; `(org-drawer ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-metadata)))
    ;;   ;;      ;; `(org-tag ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-tag)))
    ;;   ;;      ;; `(org-sexp-date ((,c :inherit modus-themes-fixed-pitch :foreground ,date-common :height 0.9)))

    ;;   ;;      ;; `(org-document-info ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-metadata-value)))
    ;;   ;;      ;; `(org-document-info-keyword ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-metadata)))
    ;;   ;;      ;; `(org-meta-line ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-metadata)))

    ;;   ;;      ;; `(org-block ((,c :inherit modus-themes-fixed-pitch :foreground ,fg-main :background ,bg-dim)))
    ;;   ;;      ;; `(org-block-begin-line ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-block :background ,bg-inactive)))
    ;;   ;;      ;; `(org-block-end-line ((,c :inherit org-block-begin-line)))

    ;;   ;;      ;; `(org-date ((,c :inherit modus-themes-fixed-pitch :foreground ,date-common)))
    ;;   ;;      ;; `(org-date-selected ((,c :inherit modus-themes-fixed-pitch :foreground ,date-common :inverse-video t)))
    ;;   ;;      ;; `(org-table ((,c :inherit modus-themes-fixed-pitch :foreground ,prose-table)))
    ;;   ;;      ;; `(org-formula ((,c :inherit modus-themes-fixed-pitch :foreground ,fnname)))
    ;;   ;;      ;; `(org-hide ((,c :inherit modus-themes-fixed-pitch :foreground ,bg-main)))
    ;;   ;;      )
    ;;   ;;     )
    ;;   ;;   )

    ;;   ;; Modus Toggle 로 불러올 때 아래 Hook 이 호출 된다.
    ;;   ;; (defun my/enable-modus-themes-variable-pitch ()
    ;;   ;;   (interactive)
    ;;   ;;   (variable-pitch-mode) ;; 모든 글꼴 변환
    ;;   ;;   (my-modus-themes-fixed-pitch-colors) ; update
    ;;   ;;   )

    ;; Modus Toggle 로 불러올 때 아래 Hook 이 호출 된다.
    ;; (defun my-modus-themes-colors ()
    ;;   (modus-themes-with-colors
    ;;     (custom-set-faces
    ;;      `(fringe ((,c :background ,bg-dim)))
    ;;      `(vterm-color-black ((,c :background "gray25" :foreground "gray25")))
    ;;      `(vterm-color-yellow ((,c :background ,yellow-intense :foreground ,yellow-intense)))
    ;;      `(translate-paragraph-highlight-face ((,c :extend t :background ,bg-red-subtle)))
    ;;      ;; `(tab-bar ((,c :inherit modus-themes-ui-variable-pitch :background ,bg-tab-bar :weight semibold)))
    ;;      ;; `(tab-line ((,c :inherit modus-themes-ui-variable-pitch :background ,bg-tab-bar :weight semibold))) ; :height 1.0
    ;;      `(jinx-misspelled ((,c :underline (:style wave :color ,magenta-intense))))
    ;;      `(treemacs-root-face ((,c :inherit org-level-2 :underline nil :weight bold :height 1.0)))
    ;;      `(treemacs-directory-face ((,c :inherit org-level-3 :height 1.0)))
    ;;      `(treemacs-file-face ((,c :inherit org-level-4 :weight regular :height 1.0)))

    ;;      ;; `(line-number ((,c :inherit ,(if modus-themes-mixed-fonts '(fixed-pitch default) 'default) :background ,bg-line-number-inactive :foreground ,fg-line-number-inactive :height 0.9)))
    ;;      ;; `(line-number-current-line ((,c :inherit (bold line-number) :background ,bg-line-number-active :foreground ,fg-line-number-active :height 0.9)))

    ;;      `(imenu-list-entry-face-0 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight bold :foreground ,fg-heading-1)))
    ;;      `(imenu-list-entry-subalist-face-0 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight bold :foreground ,fg-heading-1 :underline nil)))
    ;;      `(imenu-list-entry-face-1 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-2)))
    ;;      `(imenu-list-entry-subalist-face-1 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-2 :underline nil)))
    ;;      `(imenu-list-entry-face-2 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-3)))
    ;;      `(imenu-list-entry-subalist-face-2 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-3 :underline nil)))
    ;;      `(imenu-list-entry-face-3 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-4)))
    ;;      `(imenu-list-entry-subalist-face-3 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-4 :underline nil)))

    ;;      ;; `(org-side-tree-heading-face ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight bold :foreground ,fg-heading-1 :underline nil)))
    ;;      ;; `(highlight-indentation-current-column-face ((,c :background ,fg-alt))) ; fg-heading-0

    ;;      ;; `(doom-modeline-input-method ((,c :weight regular :foreground ,bg-main :background ,red-cooler)))
    ;;      ;; `(doom-modeline-evil-insert-state ((,c :weight regular :foreground ,bg-main :background ,green-cooler)))
    ;;      ;; `(doom-modeline-evil-visual-state ((,c :weight regular :foreground ,bg-main :background ,yellow-cooler)))
    ;;      ;; `(doom-modeline-evil-motion-state ((,c :weight regular :foreground ,bg-main :background ,blue-cooler)))
    ;;      ;; `(doom-modeline-evil-emacs-state ((,c :weight regular :foreground ,bg-main :background ,cyan-cooler)))
    ;;      ;; `(doom-modeline-evil-replace-state ((,c :weight regular :foreground ,bg-main :background ,magenta-cooler)))
    ;;      ;; `(doom-modeline-evil-normal-state (( )))
    ;;      ;; `(doom-modeline-evil-operator-state ((,c :inherit bold)))

    ;;      `(lsp-ui-doc-background ((,c (:background ,bg-dim))))
    ;;      `(lsp-ui-doc-header ((,c (:foreground ,fg-main :background ,bg-active :height 1.0))))
    ;;      `(lsp-ui-doc-url ((,c (:foreground ,fg-alt))))
    ;;      `(lsp-ui-sideline-code-action ((,c (:foreground ,fg-mark-select))))

    ;;      ;; org-mode
    ;;      `(org-level-2 ((,c :inherit modus-themes-heading-2 :underline t)))
    ;;      `(org-mode-line-clock ((,c :inherit bold :foreground ,modeline-info)))
    ;;      `(org-mode-line-clock-overrun ((,c :inherit bold :foreground ,modeline-err)))

    ;;      ;; selection 때문에 건들게 많아서 수정 안한다.
    ;;      ;; `(magit-section-heading ((,c :weight bold :height 1.2)))
    ;;      ;; `(magit-section-child-count ((,c :weight bold :height 1.2)))
    ;;      ;; `(magit-section-secondary-heading ((,c :weight semibold :height 1.2)))
    ;;      )
    ;;     )
    ;;   )

    ;; (add-hook 'modus-themes-after-load-theme-hook
    ;;           (lambda ()
    ;;             (my-modus-themes-colors)
    ;;             ;; (my-modus-themes-fixed-pitch-colors)
    ;;             ))

    ;;   )
    ;; (unless (display-graphic-p) ; terminal
    ;;   ;; (defun my-modus-themes-colors-terminal ()
    ;;   ;;   (modus-themes-with-colors
    ;;   ;;     (custom-set-faces
    ;;   ;;      `(jinx-misspelled ((,c :underline (:style line :color ,underline-warning) :foreground ,underline-warning)))
    ;;   ;;      )))
    ;;   ;; (add-hook 'modus-themes-after-load-theme-hook #'my-modus-themes-colors-terminal)
    ;;   (add-hook 'spacemacs-post-user-config-hook #'modus-themes-toggle 90)
    ;;   )


    ;;;; Fontaine (font configurations)

    ;; Read the manual: <https://protesilaos.com/emacs/fontaine>

    ;; +------------+------------+
    ;; | 일이삼사오 | 일이삼사오 |
    ;; +------------+------------+
    ;; | ABCDEFGHIJ | ABCDEFGHIJ |
    ;; +------------+------------+
    ;; | 1234567890 | 1234567890 |
    ;; +------------+------------+
    ;; | 일이삼사오 | 일이삼사오 |
    ;; | abcdefghij | abcdefghij |
    ;; +------------+------------+

    ;; terminal-mode is nil
    ;; A narrow focus package for naming font configurations and then selecting them.
    (defun jh-visual/init-fontaine ()
      (use-package fontaine
        :if window-system
        ;; :if (not (or my/remote-server *is-termux*))
        :demand t
        :init
        ;; This is defined in Emacs C code: it belongs to font settings.
        (setq x-underline-at-descent-line t)
        ;; And this is for Emacs28.
        (setq-default text-scale-remap-header-line t)

        ;; | Family                       | Shapes | Spacing | Style      | Ligatures |
        ;; |------------------------------+--------+---------+------------+-----------|
        ;; | Sarasa UI K        | Sans   | Compact | Monospaced | Yes       |
        ;; | Sarasa Mono K      | Sans   | Compact | Monospaced | Yes       |
        ;; | Sarasa Mono Slab K | Slab   | Compact | Monospaced | Yes       |
        ;; | Pretendard Variable          | Sans   |         |            | No        |

        ;; Weights :: Thin ExtraLight Light Regular Medium SemiBold Bold ExtraBold Heavy
        ;; Slopes :: Upright Oblique Italic
        ;; Width :: Normal Extended

        :config
        (unless *is-android*
          (setq fontaine-presets
                ;; 80 120, 136, 151, 180, 211
                '(
                  ;; (birdview
                  ;;  :default-height 80)
                  ;; (small
                  ;;  :default-height 120)
                  (regular)
                  ;; (medium
                  ;;  :default-height 151)
                  ;; (large
                  ;;  :default-height 180)
                  (presentation
                   :default-height 211
                   ;; :fixed-pitch-family "Sarasa Mono Slab K"
                   ;; :fixed-pitch-serif-family "Sarasa Mono Slab K"
                   :default-width extended
                   :bold-weight extrabold)
                  (t
                   ;; Following Prot’s example, keeping these for for didactic purposes.
                   :line-spacing 3
                   ;; :default-family "Sarasa Mono K"
                   ;; :default-height 136
                   :default-family "Monoplex KR Nerd"
                   :default-height 140
                   :default-weight regular
                   ;; :fixed-pitch-family "Sarasa Mono Slab K"
                   :fixed-pitch-family nil
                   :fixed-pitch-weight nil
                   :fixed-pitch-height nil
                   ;; :fixed-pitch-serif-family "Sarasa Mono Slab K" ; nil falls back to :default-family
                   :fixed-piath-serif-family nil
                   :fixed-pitch-serif-weight nil
                   :fixed-pitch-serif-height nil
                   :variable-pitch-family nil
                   ;; :variable-pitch-family "Pretendard Variable"
                   ;; :variable-pitch-family "Noto Sans KR" -- never!
                   ;; :variable-pitch-weight nil
                   ;; :variable-pitch-height 156
                   :bold-family nil
                   :bold-weight bold
                   ;; :bold-width extended
                   :italic-family nil
                   :italic-slant italic)))

          ;; Set last preset or fall back to desired style from `fontaine-presets'.
          ;; (fontaine-set-preset (or (fontaine-restore-latest-preset) 'regular))
          ;; (fontaine-set-preset 'regular)
          ;; (set-fontset-font t 'hangul (font-spec :family (face-attribute 'default :family))) ; t or nil ?

          ;; store current preset
          (defun my/fontaine-store-preset ()
            (interactive)
            (fontaine-store-latest-preset)
            ;; (message "my/fontaine-store-preset")
            )

          ;; load @ start-up
          (defun my/fontaine-load-preset ()
            (interactive)
            ;; The other side of `fontaine-restore-latest-preset'.
            ;; (add-hook 'kill-emacs-hook #'fontaine-store-latest-preset)
            (fontaine-set-preset 'regular) ; regular

            ;; 1) go default spacemacs themes
            (set-fontset-font "fontset-default" 'hangul (font-spec :family (face-attribute 'default :family)))

            ;; 2) or toggle other theme
            ;; (modus-themes-toggle) ;; Load Default Themes
            )
          (add-hook 'spacemacs-post-user-config-hook #'my/fontaine-load-preset 90)

          ;; LOAD preset fontaine
          ;; (when (file-exists-p fontaine-latest-state-file)
          ;;   (message "defer-until-after-user-config :: restore-latest-preset")
          ;;   (fontaine-set-preset (fontaine-restore-latest-preset))
          ;;   (set-fontset-font t 'hangul (font-spec :family (face-attribute 'default :family)))
          ;;   )

          ;; load @ theme change
          (defun my/fontaine-apply-current-preset ()
            (interactive)
            (fontaine-apply-current-preset)
            ;; 한글 사용 위해서 필수!
            (set-fontset-font "fontset-default" 'hangul (font-spec :family (face-attribute 'default :family))) ; default face
            ;; (set-fontset-font "fontset-default" 'hangul (font-spec :family (face-attribute 'variable-pitch :family)) nil 'append) ; for
            ;; (set-fontset-font "fontset-default" 'hangul (font-spec :family "BHGoo") nil 'append) ; 구본형체 테스트
            )

          ;; POST THEME HOOK
          (add-hook 'spacemacs-post-theme-change-hook 'my/fontaine-apply-current-preset)

          ;; 프리셋을 바꿀 경우 필수 수정 요소들
          ;; (add-hook 'fontaine-set-preset-hook 'my/theme-line-number)
          (add-hook 'fontaine-set-preset-hook 'kind-icon-reset-cache)
          )
        )
      )

    ;;;; list-unicode-display

    (defun jh-visual/init-list-unicode-display ()
      (use-package list-unicode-display :defer 15)
      )

    ;;;; popwin hook

    (defun jh-visual/pre-init-popwin ()
      (spacemacs|use-package-add-hook popwin
        :post-config
        ;; reset
        (setq popwin:special-display-config nil)

        ;; buffers that we manage
        ;; (push '("*quickrun*"             :dedicated t :position bottom :stick t :noselect t   :height 0.3) popwin:special-display-config)
        ;; (push '("*Help*"                 :dedicated t :position bottom :stick t :noselect t   :height 0.4) popwin:special-display-config)
        ;; (push '("*Process List*"         :dedicated t :position bottom :stick t :noselect nil :height 0.4) popwin:special-display-config)
        ;; (push '(dap-server-log-mode      :dedicated nil :position bottom :stick t :noselect t   :height 0.4) popwin:special-display-config)
        ;; (push '("*Shell Command Output*" :dedicated t :position bottom :stick t :noselect nil            ) popwin:special-display-config)
        ;; (push '("*Async Shell Command*"  :dedicated t :position bottom :stick t :noselect nil            ) popwin:special-display-config)
        ;; (push '("*undo-tree*"            :dedicated t :position right  :stick t :noselect nil :width   60) popwin:special-display-config)
        ;; (push '("*undo-tree Diff*"       :dedicated t :position bottom :stick t :noselect nil :height 0.3) popwin:special-display-config)
        ;; (push '("*ert*"                  :dedicated t :position bottom :stick t :noselect nil            ) popwin:special-display-config)
        ;; (push '("*grep*"                 :dedicated t :position bottom :stick t :noselect nil            ) popwin:special-display-config)
        ;; (push '("*nosetests*"            :dedicated t :position bottom :stick t :noselect nil            ) popwin:special-display-config)
        ;; (push '("^\*WoMan.+\*$" :regexp t             :position bottom                                   ) popwin:special-display-config)
        ;; (push '("*Google Translate*"     :dedicated t :position bottom :stick t :noselect t   :height 0.4) popwin:special-display-config)

        ;; DONE verification
        ;; 기존 창에 레이아웃 건들지 않고 새로운 버퍼로 등장했다가 사라지는가? dedicated t
        ;; popper 와 연동이 되는가?
        ;; popper 로 제거하거나 q 를 누를 때까지 stick 하고 있는가?
        ;; visual line mode 가 되거나. 라인 정렬이 깔끔한가
        (progn
          (push '(helpful-mode           :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
          (push '(help-mode              :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
          (push '("*Go-Translate*"       :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
          (push '("*wordreference*"      :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
          (push '("*SDCV*"               :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)

          (push '(compilation-mode       :dedicated t :position left   :stick t :noselect t :width 60) popwin:special-display-config)
          ;; (push '(dired-mode             :dedicated t :position left   :stick t :noselect nil :width 0.4) popwin:special-display-config)

          (push '(ekg-notes-mode    :dedicated t :position right :stick t :noselect nil :width 90) popwin:special-display-config)

          ;; (push '("*EKG Capture.*\\*"    :dedicated t :position right  :stick t :noselect nil :width 90) popwin:special-display-config)
          ;; (push '("*EKG Edit.*\\*"    :dedicated t :position right  :stick t :noselect nil :width 90) popwin:special-display-config)

          (push '("*eldoc*"              :dedicated t :position right  :stick t :noselect t :width 84) popwin:special-display-config)
          (push '("*devdocs-javascript*" :dedicated t :position right  :stick t :noselect t :width 84) popwin:special-display-config))


        ;; TODO 검증 할 것
        ;; (push '("*org-roam*" :dedicated t :position right :stick t :noselect nil :width 60) popwin:special-display-config)
        ;; (push '(telega-chat-mode :dedicated t :position right :stick t :noselect t :width 60) popwin:special-display-config)
        ;; (push '(telega-root-mode :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
        ;; (push '("*Dogears List*" :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)

        ;; (push '("\\*-vterm-\\*" :dedicated t :position right :stick t :noselect nil :width 70) popwin:special-display-config)
        ;; (push '(dired-mode :dedicated t :position left :stick t :noselect nil :width 40) popwin:special-display-config)
        ;; (push '("*Keyboard layout*" :dedicated t :position bottom :stick t :noselect t :height 13) popwin:special-display-config)

        (push '(flymake-diagnostics-buffer-mode :dedicated t :position bottom :stick t :noselect t :width 0.3 :height 0.3) popwin:special-display-config)
        (push '("^\\*EGLOT" :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
        (push '("*info*" :dedicated t :position right :stick t :noselect t :width 84) popwin:special-display-config)
        (push '("*eww*" :dedicated t :position right :stick t :noselect t :width 84) popwin:special-display-config)
        (push '("^\\*eldoc for" :dedicated t :position right :stick t :noselect t :width 84) popwin:special-display-config)
        (push '("^\\*Flycheck.+\\*$" :regexp t :dedicated t :position bottom :width 0.3 :height 0.3 :stick t :noselect t) popwin:special-display-config)
        ;; (push '("^\\*Backtrace\\*" :dedicated t :position bottom :stick t :noselect t :height 0.4) popwin:special-display-config)
        (push '("*lsp-documentation*" :dedicated t :position right :stick t :noselect t :width 0.3) popwin:special-display-config)
        (push '("*evil-owl*" :dedicated t :position right :stick t :noselect t :width 0.3) popwin:special-display-config)

        (push '("*Hammy Log*" :dedicated t :position bottom :stick nil :noselect t :height 0.3) popwin:special-display-config)
        (push '("*tmr-tabulated-view*" :dedicated t :position bottom :stick nil :noselect t :height 0.3) popwin:special-display-config)

        ;; (push '("*Emacs Log*" :dedicated t :position right :stick t :noselect t :width 45) popwin:special-display-config)
        ;; (push '("*Ilist*" :dedicated t :position left :stick t :noselect t :width 84) popwin:special-display-config)
        ;; (push '("*command-log*" :dedicated t :position right :stick t :noselect t :width 50) popwin:special-display-config) ; not work
        )

      ;; /garyo-dotfiles-ekg/emacs-config.org:2160
      (add-to-list 'display-buffer-alist
                   '("\\*Calendar*"
                     (display-buffer-at-bottom)))

      ;; /prot-dotfiles/emacs/.emacs.d/prot-emacs-modules/prot-emacs-window.el:55
      (add-to-list 'display-buffer-alist
                   `("\\*\\(Output\\|Register Preview\\).*"
                     (display-buffer-reuse-mode-window display-buffer-at-bottom)))

      ;; (add-to-list 'display-buffer-alist
      ;;   `("\\*\\(Calendar\\|Bookmark Annotation\\|Buffer List\\).*"
      ;;      (display-buffer-reuse-mode-window display-buffer-below-selected)
      ;;      (window-height . fit-window-to-buffer)))

      (add-to-list 'display-buffer-alist
                   ;; bottom side window
                   `("\\*Org Select\\*" ; the `org-capture' key selection
                     (display-buffer-in-side-window)
                     (dedicated . t)
                     (side . bottom)
                     (slot . 0)
                     (window-parameters . ((mode-line-format . none)))))
      (add-to-list 'display-buffer-alist
                   `("\\*Embark Actions\\*"
                     (display-buffer-reuse-mode-window display-buffer-at-bottom)
                     (window-height . fit-window-to-buffer)
                     (window-parameters . ((no-other-window . t)
                                           (mode-line-format . none)))))
      )

    ;;;; popper

    (defun jh-visual/init-popper ()
      (use-package popper
        :config
        ;; (setq popper-echo-dispatch-keys '("a" "s" "d" "f" "g" "h" "j" "k" "l"))
        (setq popper-echo-dispatch-keys '(?q ?w ?e ?r ?t ?y ?u ?i ?o ?p))
        ;; (setq popper-mode-line '(:eval (propertize "POP" 'face `(:inverse-video t))))

        (setq popper-display-control nil) ; use popwin and display-buffer-alist
        (setq popper-reference-buffers
              '("\\*Messages\\*"
                ;; "Output\\*$"
                "*cider-error*"
                ;; "*cider-doc*"
                ;; "^\\*eldoc for"
                "\\*Async-native-compile-log\\*" ; JH
                "^\\*EGLOT" ; JH
                "^\\*Flycheck.+\\*$" ; JH
                ;; treemacs-mode ; JH
                "*Go-Translate*" ; JH
                "*wordreference*" ; JH
                "*tmr-tabulated-view*" ; JH
                "*SDCV*" ; JH
                "*Dogears List*" ; JH
                "^\\*Backtrace\\*"
                "*Hammy Log*"
                ;; "*eww*"
                "*lsp-documentation*"
                "*devdocs-javascript*"
                ;; "^\\*EKG Capture"
                ekg-notes-mode
                "^\\*Ibuffer\\*" ibuffer-mode
                help-mode
                telega-chat-mode
                helpful-mode
                compilation-mode
                process-menu-mode
                special-mode
                eww-mode
                ;; "*Emacs Log*"
                ;; "*command-log*" ; JH
                ;; "*org-roam*" ; JH
                ;; org-agenda-mode ; JH
                flymake-diagnostics-buffer-mode))
        (add-to-list
         'popper-reference-buffers
         '(("^\\*Warnings\\*$" . hide)
           ("^\\*Compile-Log\\*$" . hide)
           "^\\*Matlab Help.*\\*$"
           "^\\*Messages\\*$"
           ("*typst-ts-compilation*" . hide)
           ("^\\*dash-docs-errors\\*$" . hide)
           "^\\*evil-registers\\*"
           "^\\*Apropos"
           "^Calc:"
           "^\\*eldoc\\*"
           "^\\*TeX errors\\*"
           "^\\*ielm\\*"
           "^\\*TeX Help\\*"
           "^\\*ChatGPT\\*"
           "^\\*gptel-quick\\*"
           "^\\*define-it:"
           "\\*Shell Command Output\\*"
           "\\*marginal notes\\*"
           ("\\*Async Shell Command\\*" . hide)
           "\\*Completions\\*"
           "[Oo]utput\\*"))

        ;; (global-set-key (kbd "C-`") 'popper-toggle)
        ;; (global-set-key (kbd "C-~") 'popper-kill-latest-popup)
        ;; (global-set-key (kbd "M-`") 'popper-cycle)
        ;; (global-set-key (kbd "C-M-`") 'popper-toggle-type)
        (popper-mode +1)
        (popper-echo-mode +1)
        )
      )

    ;;;; hl-todo

    ;; Highlight TODO, FIXME....
    (defun jh-visual/init-hl-todo ()
      (use-package hl-todo
        :defer 3
        :config
        ;; (message "global-hl-todo-mode")
        (global-hl-todo-mode)
        ))

    ;;;; popup

    (defun jh-visual/init-popup ()
      (use-package popup
        :defer t
        :config
        (define-key popup-menu-keymap (kbd "C-j") 'popup-next)
        (define-key popup-menu-keymap (kbd "C-k") 'popup-previous)
        (define-key popup-menu-keymap (kbd "C-n") 'popup-next)
        (define-key popup-menu-keymap (kbd "C-p") 'popup-previous)
        ))

    ;;;; Hammy -- interval timers

    (defun jh-visual/init-hammy ()
      (use-package hammy
        :init
        (setq hammy-mode-lighter-pie nil)
        ;; hammy-mode-update-mode-line-continuously
        ))

    ;;;; kind-icons

    (defun jh-visual/init-kind-icon ()
      (use-package kind-icon
        :after corfu nerd-icons
        :config
        (setq kind-icon-default-face 'corfu-default)
        (add-to-list 'corfu-margin-formatters #'kind-icon-margin-formatter)
        (when (display-graphic-p) ; gui
          (setq kind-icon-default-style '(:padding 0 :stroke 0 :margin 0 :radius 0 :height 0.9 :scale 0.9))
          )

        (unless (display-graphic-p) ; terminal
          (add-hook 'spacemacs-post-theme-change-hook 'kind-icon-reset-cache) ; move to fontaine
          (setq kind-icon-use-icons nil))
        )
      )

    ;;;; dashboard

    (defun jh-visual/init-dashboard ()
      (use-package dashboard
        :ensure t
        :after nerd-icons
        :custom
        (dashboard-center-content t)
        (dashboard-projects-backend 'projectile)
        ;; (dashboard-agenda-sort-strategy '(priority-down))
        ;; (dashboard-agenda-tags-format 'ignore)

        (dashboard-items '(
                           ;; (vocabulary)
                           ;; (recents . 5)
                           (projects . 5)
                           ;; (bookmarks . 5)
                           (agenda . 5)
                           ;; (monthly-balance)
                           (fortune)
                           ))
        (dashboard-item-generators
         '(
           ;; (monthly-balance . gopar/dashboard-ledger-monthly-balances)
           ;; (vocabulary . gopar/dashboard-insert-vocabulary)
           (fortune . my/dashboard-insert-fortune)
           ;; (recents . dashboard-insert-recents)
           (bookmarks . dashboard-insert-bookmarks)
           (projects . dashboard-insert-projects)
           (agenda . dashboard-insert-agenda)
           ;; (registers . dashboard-insert-registers)
           ))
        :init
        ;; (setq dashboard-agenda-prefix-format " %s ")
        (setq dashboard-startup-banner 'logo)

        (unless (display-graphic-p) ; terminal
          (setq dashboard-banner-logo-title "Don't be the best. Be the only. - Kevin Kelly"))

        (when (display-graphic-p) ; gui
          (setq dashboard-banner-logo-title "📜 Don't be the best. Be the only. - Kevin Kelly")
          (setq dashboard-display-icons-p t) ;; display icons on both GUI and terminal
          (setq dashboard-icon-type 'nerd-icons) ;; use `nerd-icons' package
          (setq dashboard-set-heading-icons t)
          (setq dashboard-set-file-icons t)
          ;; (setq dashboard-startup-banner (concat dotspacemacs-directory "assets/splash/gwd-light.png"))
          ;; (add-hook 'spacemacs-post-theme-change-hook (lambda () (let ((active-theme (car custom-enabled-themes)))
          ;;                                                          (setq dashboard-startup-banner (concat dotspacemacs-directory "assets/splash/"
          ;;                                                                                                 (if (eq active-theme light-theme)
          ;;                                                                                                     "gwd-light.png"
          ;;                                                                                                   "gwd-dark.png")))
          ;;                                                          (if (string-equal (buffer-name (current-buffer)) "*dashboard*")
          ;;                                                              (revert-buffer)))))
          ;; (setq dashboard-image-banner-max-width 300)
          )
        :config
        (defun gopar/dashboard-insert-vocabulary (list-size)
          (dashboard-insert-heading "Word of the Day:"
                                    nil
                                    (nerd-icons-codicon "nf-cod-library" :face 'dashboard-heading))
          (insert "\n")
          (let ((random-line nil)
                (lines nil))
            (with-temp-buffer
              (insert-file-contents (concat dotspacemacs-directory  "words"))
              (goto-char (point-min))
              (setq lines (split-string (buffer-string) "\n" t))
              (setq random-line (nth (random (length lines)) lines))
              (setq random-line (string-join (split-string random-line) " ")))
            (insert "  " random-line)))

        (defun my/dashboard-insert-fortune (list-size)
          (dashboard-insert-heading "Fortune of the Day:"
                                    nil
                                    (nerd-icons-faicon "nf-fa-pencil" :face 'dashboard-heading))
          (save-excursion
            (let* ((quotestring
                    (if (executable-find "fortune")
                        (string-join
                         (mapcar (lambda (l) (concat "\n " (string-fill l 72)))
                                 (if *is-termux*
                                     (string-lines (shell-command-to-string "fortune"))
                                   (string-lines (shell-command-to-string "fortune -c 90% advice 10% .")))))))) ;; 10% samples
              (insert "  " quotestring))))

        ;; (defun gopar/dashboard-ledger-monthly-balances (list-size)
        ;;   (interactive)
        ;;   (dashboard-insert-heading "Monthly Balance:"
        ;;                             nil
        ;;                             (all-the-icons-faicon "money"
        ;;                                                   :height 1.2
        ;;                                                   :v-adjust 0.0
        ;;                                                   :face 'dashboard-heading))
        ;;   (insert "\n")
        ;;   (let* ((categories '("Expenses:Food:Restaurants"
        ;;                        "Expenses:Food:Groceries"
        ;;                        "Expenses:Misc"))
        ;;          (current-month (format-time-string "%Y/%m"))
        ;;          (journal-file (expand-file-name "~/personal/finances/main.dat"))
        ;;          (cmd (format "ledger bal --flat --monthly --period %s %s -f %s"
        ;;                       current-month
        ;;                       (mapconcat 'identity categories " ")
        ;;                       journal-file)))

        ;;     (insert (shell-command-to-string cmd))))
        ;; (dashboard-setup-startup-hook)
        )
      )

    ;;;; Indentation-Bar
    ;;;;; indent-bars

    ;; 2023-10-02 아직 지저분하다. 나중에 만날 날이 있겠지?
    ;; (defun jh-visual/init-indent-bars ()
    ;;   (use-package indent-bars
    ;;     :if window-system
    ;;     :hook (
    ;;            (clojure-mode . indent-bars-mode)
    ;;            (clojurescript-mode . indent-bars-mode)
    ;;            (yaml-mode . indent-bars-mode)
    ;;            (toml-mode . indent-bars-mode)
    ;;            (json-mode . indent-bars-mode)
    ;;            (jsonian-mode . indent-bars-mode))
    ;;     :config
    ;;     (add-hook 'spacemacs-post-theme-change-hook 'indent-bars-reset)

    ;;     (setq ; terminal custom
    ;;      indent-bars-prefer-character t
    ;;      indent-bars-color '(highlight :face-bg t :blend 0.45) ; 0.75
    ;;      indent-bars-color-by-depth '(:regexp "outline-\\([0-9]+\\)" :blend 1)
    ;;      indent-bars-unspecified-fg-color "white"
    ;;      indent-bars-unspecified-bg-color "black")

    ;;     ;; org-mode-hook
    ;;     (add-hook 'org-mode-local-vars-hook
    ;;               (defun +indent-bars-disable-maybe-h ()
    ;;                 (and indent-bars-mode
    ;;                      (bound-and-true-p org-indent-mode)
    ;;                      (indent-bars-mode -1))))
    ;;     )
    ;;   )

    ;;;;; DONT highlight-indentation

    ;; (defun jh-visual/init-highlight-indentation ()
    ;;   (use-package highlight-indentation
    ;;     :if window-system
    ;;     :ensure t
    ;;     :hook (;; (prog-mode . highlight-indentation-mode)
    ;;            (clojure-mode . highlight-indentation-current-column-mode)
    ;;            (clojurescript-mode . highlight-indentation-current-column-mode)
    ;;            (json-mode . highlight-indentation-current-column-mode)
    ;;            (json-mode . highlight-indentation-current-column-mode)
    ;;            (yaml-mode . highlight-indentation-current-column-mode)
    ;;            )
    ;;     ;; :config
    ;;     ;; (set-face-attribute 'highlight-indentation-face nil :background "black")
    ;;     )
    ;;   )

    ;;; packages.el ends here

    ```


#### <span class="section-num">3.4.3</span> The `jh-visual` funcs.el {#h:efab0a3d-c312-4773-aded-e2a00a9bdd6b}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; Theme framework

(defun my/doom-p ()
  (seq-find (lambda (x) (string-match-p (rx bos "doom") (symbol-name x)))
            custom-enabled-themes))

(defun my/modus-p ()
  (seq-find (lambda (x) (string-match-p (rx bos "modus") (symbol-name x)))
            custom-enabled-themes))

(defun my/ef-p ()
  (seq-find (lambda (x) (string-match-p (rx bos "ef") (symbol-name x)))
            custom-enabled-themes))

(defun my/light-p ()
  (ct-light-p (my/color-value 'bg)))

(defun my/dark-p ()
  (not (my/light-p)))

(defconst my/theme-override
  '((doom-palenight
     (red . "#f07178"))))

(defvar my/alpha-for-light 7)

(defun my/doom-color (color)
  (when (doom-color 'bg)
    (let ((override (alist-get (my/doom-p) my/theme-override))
          (color-name (symbol-name color))
          (is-light (ct-light-p (doom-color 'bg))))
      (or
       (alist-get color override)
       (cond
        ((eq 'black color)
         (if is-light (doom-color 'fg) (doom-color 'bg)))
        ((eq 'white color)
         (if is-light (doom-color 'bg) (doom-color 'fg)))
        ((eq 'border color)
         (if is-light (doom-color 'base0) (doom-color 'base8)))
        ((string-match-p (rx bos "light-") color-name)
         (ct-edit-hsl-l-inc (my/doom-color (intern (substring color-name 6)))
                            my/alpha-for-light))
        (t (doom-color color)))))))

(defun my/modus-get-base (color)
  (let ((base-value (string-to-number (substring (symbol-name color) 4 5)))
        (base-start (cadr (assoc 'bg-main (modus-themes--current-theme-palette))))
        (base-end (cadr (assoc 'fg-dim (modus-themes--current-theme-palette)))))
    (nth base-value (ct-gradient 9 base-start base-end t))))

(defun my/prot-color (color palette)
  (let ((is-light (ct-light-p (cadr (assoc 'bg-main palette)))))
    (cond
     ((member color '(black white light-black light-white))
      (let ((bg-main (cadr (assoc 'bg-main palette)))
            (fg-main (cadr (assoc 'fg-main palette))))
        (pcase color
          ('black (if is-light fg-main bg-main))
          ('white (if is-light bg-main fg-main))
          ('light-black (ct-edit-hsl-l-inc
                         (if is-light fg-main bg-main)
                         15))
          ('light-white (ct-edit-hsl-l-inc
                         (if is-light bg-main fg-main)
                         15)))))
     ((or (eq color 'bg))
      (cadr (assoc 'bg-main palette)))
     ((or (eq color 'fg))
      (cadr (assoc 'fg-main palette)))
     ((eq color 'bg-alt)
      (cadr (assoc 'bg-dim palette)))
     ((eq color 'bg-other)
      (cadr (assoc 'bg-inactive palette)))
     ((eq color 'violet)
      (cadr (assoc 'magenta-cooler palette)))
     ((string-match-p (rx bos "base" digit) (symbol-name color))
      (my/modus-get-base color))
     ((string-match-p (rx bos "dark-") (symbol-name color))
      (cadr (assoc (intern (format "%s-cooler" (substring (symbol-name color) 5)))
                   palette)))
     ((eq color 'grey)
      (my/modus-get-base 'base5))
     ((string-match-p (rx bos "light-") (symbol-name color))
      (or
       (cadr (assoc (intern (format "%s-intense" (substring (symbol-name color) 6))) palette))
       (cadr (assoc (intern (format "bg-%s-intense" (substring (symbol-name color) 6))) palette))))
     (t (cadr (assoc color palette))))))

(defun my/modus-color (color)
  (my/prot-color color (modus-themes--current-theme-palette)))

(defun my/ef-color (color)
  (my/prot-color color (ef-themes--current-theme-palette)))

(defconst my/test-colors-list
  '(black red green yellow blue magenta cyan white light-black
          light-red light-green light-yellow light-blue light-magenta
          light-cyan light-white bg fg violet grey base0 base1 base2
          base3 base4 base5 base6 base7 base8 border bg-alt))

(defun my/test-colors ()
  (interactive)
  (let ((buf (generate-new-buffer "*colors-test*")))
    (with-current-buffer buf
      (insert (format "%-20s %-10s %-10s %-10s" "Color" "Doom" "Modus" "Ef") "\n")
      (cl-loop for color in my/test-colors-list
               do (insert
                   (format "%-20s %-10s %-10s %-10s\n"
                           (prin1-to-string color)
                           (my/doom-color color)
                           (my/modus-color color)
                           (my/ef-color color))))
      (special-mode)
      (rainbow-mode))
    (switch-to-buffer buf)))

(defun my/color-value (color)
  (cond
   ((stringp color) (my/color-value (intern color)))
   ((eq color 'bg-other)
    (or (my/color-value 'bg-dim)
        (let ((color (my/color-value 'bg)))
          (if (ct-light-p color)
              (ct-edit-hsl-l-dec color 2)
            (ct-edit-hsl-l-dec color 3)))))
   ((my/doom-p) (my/doom-color color))
   ((my/modus-p) (my/modus-color color))
   ((my/ef-p) (my/ef-color color))))

(deftheme my-theme-1)

(defvar my/my-theme-update-color-params nil)

(defmacro my/use-colors (&rest data)
  `(progn
     ,@(cl-loop for i in data collect
                `(setf (alist-get ',(car i) my/my-theme-update-color-params)
                       (list ,@(cl-loop for (key value) on (cdr i) by #'cddr
                                        append `(,key ',value)))))
     (when (and (or (my/doom-p) (my/modus-p)) my/emacs-started)
       (my/update-my-theme))))

(defun my/update-my-theme (&rest _)
  (interactive)
  (message "Updating themes...")
  (cl-loop for (face . values) in my/my-theme-update-color-params
           do (custom-theme-set-faces
               'my-theme-1
               `(,face ((t ,@(cl-loop for (key value) on values by #'cddr
                                      collect key
                                      collect (eval value)))))))
  (enable-theme 'my-theme-1))

(unless *is-termux*
  (add-hook 'emacs-startup-hook #'my/update-my-theme)

  ;; for spacemacs style
  ;; ;; (advice-add 'load-theme :after #'my/update-my-theme)
  (add-hook 'spacemacs-post-theme-change-hook 'my/update-my-theme)
  )

(my/use-colors
 (tab-bar-tab :background (my/color-value 'bg)
              :foreground (my/color-value 'yellow)
              :underline (my/color-value 'yellow)
              :weight 'semibold)
 (tab-bar :background (my/color-value 'bg) :foreground (my/color-value 'fg) :weight 'semibold)

 (org-mode-line-clock :inherit 'success :weight 'semibold :foreground (my/color-value 'blue))
 (org-mode-line-clock-overrun :inherit 'error :weight 'semibold :foreground (my/color-value 'red))
 (org-default :foreground (my/color-value 'fg))

 (lsp-ui-doc-background :background (my/color-value 'bg-alt))
 (lsp-ui-doc-header :background (my/color-value 'bg) :foreground (my/color-value 'fg) :height 1.0 :weight 'semibold)
 (lsp-ui-doc-url :foreground (my/color-value 'light-blue))
 (lsp-ui-sideline-code-action :foreground (my/color-value 'violet))
 (consult-notes-sep :foreground (my/color-value 'fg))

 ;; (doom-modeline-input-method :foreground (my/color-value 'bg) :background (my/color-value 'red) :weight 'regular)
 ;; (doom-modeline-evil-insert-state :foreground (my/color-value 'bg) :background (my/color-value 'green) :weight 'regular)
 ;; (doom-modeline-evil-visual-state :foreground (my/color-value 'bg) :background (my/color-value 'yellow) :weight 'regular)
 ;; (doom-modeline-evil-motion-state :foreground (my/color-value 'bg) :background (my/color-value 'blue) :weight 'regular)
 ;; (doom-modeline-evil-emacs-state :foreground (my/color-value 'bg) :background (my/color-value 'cyan) :weight 'regular)
 ;; (doom-modeline-evil-replace-state :foreground (my/color-value 'bg) :background (my/color-value 'magenta) :weight 'regular)

 (treemacs-root-face :inherit 'font-lock-string-face :weight 'bold :height 1.0)

 (magit-section-secondary-heading :foreground (my/color-value 'blue) :weight 'bold)
 )

(my/use-colors
 (auto-dim-other-buffers-face
  :background (my/color-value 'bg-other)))

(defun remove-items (x y)
  (setq y (cl-remove-if (lambda (item) (memq item x)) y))
  y)

(setq my/color-themes '(
                        doom-monokai-spectrum
                        doom-dracula
                        ))

(setq my/white-themes '(
                        doom-one-light
                        ))
(setq my/black-themes '(
                        ef-maris-dark
                        modus-vivendi-tinted
                        ))
(setq my/colorless-thems
      '(
        doom-homage-white
        doom-plain
        doom-plain-dark
        ))

(setq my/themes-blacklisted '(
                              spacemacs-light
                              spacemacs-dark
                              spacemacs
                              ef-tritanopia-light
                              ef-tritanopia-dark
                              ef-deuteranopia-light
                              ef-deuteranopia-dark
                              modus-operandi-deuteranopia
                              modus-vivendi-deuteranopia
                              modus-operandi-tritanopia
                              modus-vivendi-tritanopia
                              leuven
                              leuven-dark
                              manoj-dark
                              tango
                              wombat
                              adwaita
                              tsdh-dark
                              dichromacy
                              light-blue
                              misterioso
                              tango-dark
                              tsdh-light
                              wheatgrass
                              whiteboard
                              deeper-blue))
(setq consult-themes (remove-items my/themes-blacklisted (custom-available-themes)))

;;;###autoload
(defun my/switch-theme (theme)
  (interactive
   (list (intern (completing-read "Load custom theme: "
                                  (mapcar #'symbol-name
                                          (remove-items  my/themes-blacklisted (custom-available-themes)) ; (custom-available-themes)
                                          )))))
  (cl-loop for enabled-theme in custom-enabled-themes
           if (not (or (eq enabled-theme 'my-theme-1)
                       (eq enabled-theme theme)))
           do (disable-theme enabled-theme))
  (load-theme theme t)

  (when (string-match-p "doom-" (format "%s" theme))
    ;; (message "%s" (format "%s" theme))
    (custom-set-faces

     `(imenu-list-entry-face-0 ((t (:inherit outline-1 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-subalist-face-0 ((t (:inherit outline-1 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-face-1 ((t (:inherit outline-2 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-subalist-face-1 ((t (:inherit outline-2 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-face-2 ((t (:inherit outline-3 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-subalist-face-2 ((t (:inherit outline-3 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-face-3 ((t (:inherit outline-4 :width narrow :weight bold :height 0.9))))
     `(imenu-list-entry-subalist-face-3 ((t (:inherit outline-4 :width narrow :weight bold :height 0.9))))

     `(ekg-notes-mode-title ((t (:inherit outline-7 :weight bold :height 1.3))))
     ;; ;; (ekg-tag :background (my/color-value 'bg-alt) :box t)
     `(ekg-title ((t (:inherit outline-0 :weight bold :height 1.0 :underline t))))
     ;; `(ekg-metadata ((t (:inherit highlight :height 1.1 ))))

     `(denote-faces-link ((t (:inherit link :weight bold :slant italic))))
     ))

  (when (string-match-p "ef-" (format "%s" theme))
    ;; (message "%s" (format "%s" theme))
    (ef-themes-with-colors
      (custom-set-faces
       `(imenu-list-entry-face-0 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-1 :height 0.9)))
       `(imenu-list-entry-subalist-face-0 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-1 :underline nil :height 0.9)))
       `(imenu-list-entry-face-1 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-2 :height 0.9)))
       `(imenu-list-entry-subalist-face-1 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight bold :foreground ,rainbow-2 :underline nil :height 0.9)))
       `(imenu-list-entry-face-2 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-3 :height 0.9)))
       `(imenu-list-entry-subalist-face-2 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-3 :underline nil :height 0.9)))
       `(imenu-list-entry-face-3 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-4 :height 0.9)))
       `(imenu-list-entry-subalist-face-3 ((,c :inherit ef-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,rainbow-4 :underline nil :height 0.9)))
       ;; `(org-level-2 ((,c :inherit ef-themes-heading-2 :extend nil :underline t)))

       `(ekg-notes-mode-title ((,c :inherit outline-7 :weight bold :height 1.3)))
       ;; (ekg-tag :background (my/color-value 'bg-alt) :box t)
       `(ekg-title ((,c :inherit outline--0 :weight bold :height 1.0 :underline t)))
       ;; `(ekg-metadata ((,c :inherit highlight :height 1.1)))

       `(denote-faces-link ((,c :inherit link :foreground ,blue-cooler :weight bold)))
       )))

  (when (string-match-p "modus-" (format "%s"theme))
    ;; (message "%s" (format "%s" theme))
    (modus-themes-with-colors
      (custom-set-faces
       `(imenu-list-entry-face-0 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight bold :foreground ,fg-heading-1 :height 0.9)))
       `(imenu-list-entry-subalist-face-0 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight bold :foreground ,fg-heading-1 :underline nil :height 0.9)))
       `(imenu-list-entry-face-1 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-2 :height 0.9)))
       `(imenu-list-entry-subalist-face-1 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-2 :underline nil :height 0.9)))
       `(imenu-list-entry-face-2 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-3 :height 0.9)))
       `(imenu-list-entry-subalist-face-2 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-3 :underline nil :height 0.9)))
       `(imenu-list-entry-face-3 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-4 :height 0.9)))
       `(imenu-list-entry-subalist-face-3 ((,c :inherit modus-themes-ui-variable-pitch :width narrow :weight semibold :foreground ,fg-heading-4 :underline nil :height 0.9)))
       ;; `(org-level-2 ((,c :inherit modus-themes-heading-2 :extend nil :underline t)))

       `(ekg-notes-mode-title ((,c :inherit outline-7 :weight bold :height 1.3)))
       ;; (ekg-tag :background (my/color-value 'bg-alt) :box t)
       `(ekg-title ((,c :inherit outline-0 :weight bold :height 1.0 :underline t)))
       ;; `(ekg-metadata ((,c :inherit highlight :height 1.0)))

       `(denote-faces-link ((,c :inherit link :foreground ,blue-intense :weight bold)))
       ))
    )

  ;; (when current-prefix-arg
  ;;   (my/regenerate-desktop))

  (kind-icon-reset-cache)

  ) ; end-of defun

;; (with-eval-after-load 'ansi-color
;;   (my/use-colors
;;    (ansi-color-black
;;     :foreground (my/color-value 'base2) :background (my/color-value 'base0))
;;    (ansi-color-red
;;     :foreground (my/color-value 'red) :background (my/color-value 'red))
;;    (ansi-color-green
;;     :foreground (my/color-value 'green) :background (my/color-value 'green))
;;    (ansi-color-yellow
;;     :foreground (my/color-value 'yellow) :background (my/color-value 'yellow))
;;    (ansi-color-blue
;;     :foreground (my/color-value 'dark-blue) :background (my/color-value 'dark-blue))
;;    (ansi-color-magenta
;;     :foreground (my/color-value 'violet) :background (my/color-value 'violet))
;;    (ansi-color-cyan
;;     :foreground (my/color-value 'dark-cyan) :background (my/color-value 'dark-cyan))
;;    (ansi-color-white
;;     :foreground (my/color-value 'base8) :background (my/color-value 'base8))
;;    (ansi-color-bright-black
;;     :foreground (my/color-value 'base5) :background (my/color-value 'base5))
;;    (ansi-color-bright-red
;;     :foreground (my/color-value 'orange) :background (my/color-value 'orange))
;;    (ansi-color-bright-green
;;     :foreground (my/color-value 'teal) :background (my/color-value 'teal))
;;    (ansi-color-bright-yellow
;;     :foreground (my/color-value 'yellow) :background (my/color-value 'yellow))
;;    (ansi-color-bright-blue
;;     :foreground (my/color-value 'blue) :background (my/color-value 'blue))
;;    (ansi-color-bright-magenta
;;     :foreground (my/color-value 'magenta) :background (my/color-value 'magenta))
;;    (ansi-color-bright-cyan
;;     :foreground (my/color-value 'cyan) :background (my/color-value 'cyan))
;;    (ansi-color-bright-white
;;     :foreground (my/color-value 'fg) :background (my/color-value 'fg))))


;;; default theme
(setq my/default-theme (let ((hr (nth 2 (decode-time))))
                         (if (or (< hr 7) (< 19 hr))    ; between 8 PM and 7 AM
                             '(modus-vivendi-tinted modus-operandi) ; load dark theme first
                           '(modus-operandi modus-vivendi-tinted))))

;;; toggle-line-spacing

(defun my/toggle-line-spacing ()
  "Toggle line spacing between no extra space to extra half line height.
  URL `http://ergoemacs.org/emacs/emacs_toggle_line_spacing.html'
  Version 2015-12-17"
  (interactive)
  (if (null line-spacing)
      ;; (setq line-spacing 0.5) ; add 0.5 height between lines
      (setq line-spacing 1) ; add 0.5 height between lines
    (setq line-spacing nil)   ; no extra heigh between lines
    )
  (redraw-frame (selected-frame)))

;;; text-font-scale up/down

(defun my/text-font-scale-up ()
  (interactive)
  (text-scale-increase 1)
  )
(defun my/text-font-scale-down ()
  (interactive)
  (text-scale-decrease 1)
  )

;;; list-fonts-display

(defun my/list-fonts-display (&optional matching)
  "Display a list of font-families available via font-config, in a new buffer.
   If the optional argument MATCHING is non-nil, only
   font families matching that regexp are displayed;
   interactively, a prefix argument will prompt for the
   regexp.  The name of each font family is displayed
   using that family, as well as in the default font (to
   handle the case where a font cannot be used to display
   its own name)."
  (interactive
   (list
    (and current-prefix-arg
         (read-string "Display font families matching regexp: "))))
  (let (families)
    (with-temp-buffer
      (shell-command "fc-list : family" t)
      (goto-char (point-min))
      (while (not (eobp))
        (let ((fam (buffer-substring (line-beginning-position)
                                     (line-end-position))))
          (when (or (null matching) (string-match matching fam))
            (push fam families)))
        (forward-line)))
    (setq families
          (sort families
                (lambda (x y) (string-lessp (downcase x) (downcase y)))))
    (let ((buf (get-buffer-create "*Font Families*")))
      (with-current-buffer buf
        (erase-buffer)
        (dolist (family families)
          ;; We need to pick one of the comma-separated names to
          ;; actually use the font; choose the longest one because some
          ;; fonts have ambiguous general names as well as specific
          ;; ones.
          (let ((family-name
                 (car (sort (split-string family ",")
                            (lambda (x y) (> (length x) (length y))))))
                (nice-family (replace-regexp-in-string "," ", " family)))
            (insert (concat (propertize nice-family
                                        'face (list :family family-name))
                            " (" nice-family ")"))
            (newline)))
        (goto-char (point-min)))
      (display-buffer buf))))


;;; funcs.el ends here

```


#### <span class="section-num">3.4.4</span> The `jh-visual` config.el {#h:4425477d-467d-4bf8-be1c-a35c00c8b831}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; Base-config

(defvar show-keyboard-layout nil
  "If non nil, show keyboard layout in special buffer.")

(setq default-input-method "korean-hangul")
(setq default-transient-input-method "TeX")
(set-language-environment "Korean")
(set-keyboard-coding-system 'utf-8)
(setq locale-coding-system  'utf-8)
(prefer-coding-system 'utf-8)
(set-charset-priority 'unicode)
(set-default-coding-systems 'utf-8)
(set-terminal-coding-system 'utf-8)
(setq-default buffer-file-coding-system 'utf-8-unix)
(unless (spacemacs/system-is-mswindows)
  (set-selection-coding-system 'utf-8))

(setq coding-system-for-read 'utf-8)
(setq coding-system-for-write 'utf-8)

;; Treat clipboard input as UTF-8 string first; compound text next, etc.
(setq x-select-request-type '(UTF8_STRING COMPOUND_TEXT TEXT STRING))

;; terminal-mode is nil
;; (setq-default line-spacing 2) ; use fontaine

;; 날짜 표시를 영어로한다. org mode에서 time stamp 날짜에 영향을 준다.
(setq system-time-locale "C")
;; (setenv "LANG" "en_US.UTF-8")
;; (setenv "LC_ALL" "en_US.UTF-8")

(setq input-method-verbose-flag nil
      input-method-highlight-flag nil)

;;; Emoji and Symbol

(defun jh-visual/emoji-set-font ()
  (when (display-graphic-p) ; gui
    ;; (set-fontset-font t 'emoji (font-spec :family "Apple Color Emoji") nil 'prepend)
    (set-fontset-font t 'emoji (font-spec :family "Noto Color Emoji") nil)
    (set-fontset-font t 'emoji (font-spec :family "Noto Emoji") nil 'prepend) ; Top
    )
  (unless (display-graphic-p) ; terminal
    (set-fontset-font "fontset-default" 'emoji (font-spec :family "Noto Emoji") nil 'prepend) ; default face
    )

  (set-fontset-font t 'symbol (font-spec :family "Symbola") nil 'prepend)
  (set-fontset-font t 'symbol (font-spec :family "Noto Sans Symbols 2") nil 'prepend)
  (set-fontset-font t 'symbol (font-spec :family "Noto Sans Symbols") nil 'prepend)

  ;; 2023-09-14 심볼에는 컬러 빼자. 터미널과 호환성 유지 차원
  ;; (set-fontset-font t 'symbol (font-spec :family "Noto Color Emoji") nil 'prepend) ; Top
  )

(unless *is-termux*
  (add-hook 'after-init-hook #'jh-visual/emoji-set-font)
  )

;;; theme--tweaks-h

;; (defun my/theme-line-number (&optional _)
;;   (interactive)
;;   "Use smaller font (90% of the default) for line numbers in graphic mode."
;;   (when (display-graphic-p)
;;     (set-face-attribute
;;      'line-number nil
;;      :background (face-attribute 'default :background)
;;      :height (truncate (* 0.90 (face-attribute 'default :height)))
;;      :weight 'semi-light)
;;     (set-face-attribute
;;      'line-number-current-line nil
;;      :height (truncate (* 0.90 (face-attribute 'default :height)))
;;      :weight 'bold)))

;; (spacemacs|do-after-display-system-init
;;  (my/theme-line-number))

;; ;; (add-hook 'spacemacs-post-theme-change-hook #'my/theme-line-number)
;; ;; (add-hook 'after-init-hook #'+theme--tweaks-h) ; not work on spacemacs

;; copy from 'asok-dot-spacemacs/.spacemacs:975'
;; (defadvice load-theme
;;     (before theme-dont-propagate activate)
;;   (mapc #'disable-theme custom-enabled-themes))

;;; goto-address-mode

(setq goto-address-url-face 'link
      goto-address-url-mouse-face 'highlight
      goto-address-mail-face nil
      goto-address-mail-mouse-face 'highlight)

;;; display-buffer-alist

(defun my/load-global-mode-string ()
  (interactive)
  ;; (message "my/load-global-mode-string")
  (when (not (bound-and-true-p display-time-mode))
    (display-time-mode t))

  (setq global-mode-string (remove 'display-time-string global-mode-string))
  (setq global-mode-string '("" celestial-mode-line-string display-time-string))

  (when (display-graphic-p) ; gui
    (hammy-mode))
  )

(unless *is-termux*
  (add-hook 'spacemacs-post-user-config-hook #'my/load-global-mode-string 90))

;;; config.el ends here

```


#### <span class="section-num">3.4.5</span> The `jh-visual` keybindings.el {#h:ea23046d-af69-45fa-ab7a-1ae10be71950}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

(global-set-key (kbd "C-+") 'my/text-font-scale-up)
(global-set-key (kbd "C-_") 'my/text-font-scale-down)

(spacemacs/set-leader-keys "z+" 'my/text-font-scale-up)
(spacemacs/set-leader-keys "z_" 'my/text-font-scale-down)

(spacemacs/set-leader-keys "zf" 'fontaine-set-preset)
(spacemacs/set-leader-keys "zF" 'fontaine-set-face-font)

(spacemacs/set-leader-keys ";" 'popper-toggle)
(spacemacs/set-leader-keys ":" 'popper-kill-latest-popup)

;; (global-set-key [?\C-\\] 'jh-visual//change-input-method)
(global-set-key (kbd "<S-SPC>") 'toggle-input-method)
;; (global-set-key (kbd "<Alt_R>") 'toggle-input-method)
(global-set-key (kbd "<Hangul>") 'toggle-input-method)
;; (global-unset-key (kbd "S-SPC"))

(spacemacs/set-leader-keys "ii" 'insert-char)
(spacemacs/set-leader-keys "iu" 'list-unicode-display)
(when (> emacs-major-version 28) ; emacs 29
  (spacemacs/set-leader-keys "ie" #'emoji-search)
  (spacemacs/set-leader-keys "iE" #'emoji-list)
  )

;; (spacemacs/set-leader-keys "T," 'ef-themes-load-random-light)
;; (spacemacs/set-leader-keys "T." 'ef-themes-load-random-dark)
;; (spacemacs/set-leader-keys "T<" 'ef-themes-select)
;; (spacemacs/set-leader-keys "T>" 'modus-themes-select)

;; (spacemacs/set-leader-keys "T;" 'ef-themes-toggle)
;; (spacemacs/set-leader-keys "T/" 'modus-themes-toggle)

(spacemacs/set-leader-keys "Tl" 'my/toggle-line-spacing)

;; hammy
(spacemacs/set-leader-keys "oh" 'hammy-next)
(spacemacs/set-leader-keys "oH" 'hammy-start)

;; (define-key popper-mode-map (kbd "M-'") 'popper-cycle)

;;; keybindings.el ends here

```


### <span class="section-num">3.5</span> The `jh-workspace` layer {#h:a6d20fd4-d411-45cf-921b-2d427b4095f7}




#### <span class="section-num">3.5.1</span> The `jh-workspace` layer.el {#h:9bcfb8d3-bf79-4121-bc12-6a6ff3941ddb}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;; workspace :: layouts navigation purpose project

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(

;;;; Spacemacs Layer

   (spacemacs-layouts :variables
                      ;; 현재 레이아웃의 버퍼 사이만 전환
                      spacemacs-layouts-restrict-spc-tab t ; default nil
                      ;; persp-autokill-buffer-on-remove 'kill-weak ; default nil
                      )

   ;; ace-link
   ;; ace-window
   ;; auto-highlight-symbol
   ;; centered-cursor-mode
   ;; open-junk-file
   (spacemacs-navigation :packages (not paradox symbol-overlay))

   spacemacs-purpose ; window-purpose

   spacemacs-project ; projectile

;;;; Workspace

   ;; Grouping buffers with ibuffer
   ;; ibuffer :: SPC b I, C-x C-b
   ;; refresh :: g r, fliter group :: g j/k, ][, TAB/S-TAB, M-n/p
   (ibuffer
    :variables
    ;; ibuffer-old-time 8 ; buffer considered old after that many hours
    ibuffer-show-empty-filter-groups nil
    ibuffer-group-buffers-by 'projects)

;;;; Navigation

   ;; fasd

   bm ; bookmark

   ;; ;; outshine -- replaced by 'outli'

   ;; Visual file manager - `SPC p t'
   ;; treemacs-no-png-images t removes file and directory icons
   ;; 2023-09-01 Delete treemacs-magit treemacs-persp treemacs-projectile conflict lsp-mode
   (treemacs
    :packages (not treemacs-magit treemacs-persp treemacs-projectile)
    :variables
    treemacs-position 'left
    treemacs-width 45
    treemacs-imenu-scope 'current-project
    ;; treemacs-indentation 1
    treemacs-lock-width t
    treemacs-space-between-root-nodes nil ; spacing in treemacs views
    treemacs-fringe-indicator-mode nil

    treemacs-use-all-the-icons-theme nil ; important
    treemacs-use-icons-dired nil ; important
    )

;;;; Version Control

   ;; SPC g s opens Magit git client full screen (q restores previous layout)
   ;; show word-granularity differences in current diff hunk
   ;;      git-enable-magit-gitflow-plugin t
   (git
    :variables
    forge-database-connector 'sqlite-builtin
    git-enable-magit-delta-plugin t
    git-enable-magit-todos-plugin t)

   ;; Highlight changes in buffers
   ;; SPC g . transient state for navigating changes
   (version-control
    :variables
    ;; https://github.com/syl20bnr/spacemacs/pull/16156/commits/435f4ae800f4229248fb5549cdbd84d303e677cc
    ;; version-control-diff-tool 'diff-hl ; 2023-10-23 enable
    ;; version-control-diff-side 'left
    ;; 아래에 margin 을 켜야 git-gutter 가 mode 활성화 된다
    version-control-global-margin t

    version-control-diff-tool 'git-gutter ; simpler
    git-gutter:update-interval 5 ; spacemacs 2
    git-gutter:hide-gutter nil
    )
   )
 )

```


#### <span class="section-num">3.5.2</span> The `jh-workspace` packages.el {#h:3ca3dcf9-40e3-4ce3-9637-30acd620ac7d}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code: Package Lists

    (defconst jh-workspace-packages
      '(
    ;;;; Navigation

        ace-window
        winum
        dumb-jump
        goto-last-change
        binky

        (outli :location (recipe :fetcher github :repo "jdtsmith/outli"))
        (loccur :location (recipe :fetcher github :repo "thanhvg/loccur"))

        burly

        dired-subtree
        dired-narrow
        dired-filter

        dired-open
        dired-git-info
        dired-preview

        ;; dired-hide-dotfiles

        ;; dired-recent
        ;; dired-rsync
        ;; org-side-tree

    ;;;; Workspace

        tab-bar
        persp-mode
        nerd-icons-ibuffer
        neotree
        tabspaces

        ;; side-hustle ; imenu

    ;;;; Project

        projectile
        ;; git-commit
        ;; magit
        ;; forge
        ;; git-timemachine

        ;; magit-imerge

        consult-git-log-grep
        consult-projectile

        blamer

        git-cliff

        ;; (consult-gh :location (recipe :fetcher github :repo "armindarvish/consult-gh"))

        ;; magit-stats
        ;; code-review
        ;; gpt-commit
        )
      )
    ```

<!--list-separator-->

2.  Configurations

    ```elisp


    ;;; Navigation
    ;;;; winum

    (defun jh-workspace/post-init-winum ()
      (defun my/winum-assign-custom ()
        (cond
         ;; 0 treemacs, 9 minibuffer, 8 imenu-list
         ((equal (buffer-name) "*Ilist*") 8)
         ((equal (buffer-name) "*Flycheck errors*") 7)
         ((equal (buffer-name) "*Calculator*") 6))
        )

      (set-face-attribute 'winum-face nil :weight 'bold)
      (add-to-list 'winum-assign-functions #'my/winum-assign-custom)

      (define-key winum-keymap
                  [remap winum-select-window-7] #'spacemacs/neotree-smart-focus)
      (define-key winum-keymap
                  [remap winum-select-window-8] #'spacemacs/imenu-list-smart-focus)
      (define-key winum-keymap
                  [remap winum-select-window-9] #'spacemacs/switch-to-minibuffer-window)

      (setq winum-scope                      'frame-local
            winum-auto-assign-0-to-minibuffer nil
            winum-reverse-frame-list          nil)
      (winum-mode 1)
      )

    ;;;; ace-window

    (defun jh-workspace/post-init-ace-window ()
      (setq aw-scope 'frame
            aw-minibuffer-flag t)
      ;; (ace-window-display-mode 1)
      (global-set-key (kbd "M-g a") 'ace-swap-window)
      )

    ;;;; goto-last-change

    (defun jh-workspace/init-goto-last-change ()
      (require 'goto-last-change))

    ;; goto-chg 패키지에서 아래 함수 제공
    ;; evil-goto-last-change-reverse (g ,)
    ;; evil-goto-last-change (g ;)

    ;;;; dumb-jump

    (defun jh-workspace/init-dumb-jump ()
      (use-package dumb-jump
        :defer 2
        :commands (dumb-jump-hydra/body)
        :custom  (dumb-jump-selector 'completing-read)
        :init
        ;; Use as xref backend
        (with-eval-after-load 'xref
          (add-hook 'xref-backend-functions #'dumb-jump-xref-activate 101)
          )
        :config
        ;; Define Hydra keybinding (from the repo's examples)
        (defhydra dumb-jump-hydra
          (:color blue :hint nil :foreign-keys warn) ;; :exit t :quit-key "q")
          "
    [Dumb Jump]                                                                         [_q_] quit
      ├─────────────────────────────────────────────────────────────────────────────────────────╮
      │  [_j_] Go          [_o_] Go other window    [_e_] Go external   [_x_] Go external other window  │
      │  [_i_] Go prompt   [_l_] Quici look         [_b_] Back                                        │
      ╰─────────────────────────────────────────────────────────────────────────────────────────╯
    "
          ("j" dumb-jump-go)
          ("o" dumb-jump-go-other-window)
          ("e" dumb-jump-go-prefer-external)
          ("x" dumb-jump-go-prefer-external-other-window)
          ("i" dumb-jump-go-prompt)
          ("l" dumb-jump-quick-look)
          ("b" dumb-jump-back)
          ("q" nil :color blue))
        )
      )

    ;;;; bulry

    (defun jh-workspace/init-burly ()
      (use-package burly
        :after tab-bar
        :init
        (setq burly-tabs-mode nil)
        )
      )

    ;;;; loccur

    (defun jh-workspace/init-loccur ()
      (use-package loccur
        :defer 10
        :config
        (spacemacs/declare-prefix "sv"  "loccur")
        (spacemacs/set-leader-keys
          "svv" 'loccur-current
          "svl" 'loccur-previous-match
          "svo" 'loccur)
        )
      )
    ;;;; TODO Prot's Dired

    ;; ;; NOTE 2021-05-10: I do not use `find-dired' and related commands
    ;; ;; because there are other tools that offer a better interface, such
    ;; ;; as `consult-find', `consult-grep', `project-find-file',
    ;; ;; `project-find-regexp', `prot-vc-git-grep'.
    ;; (prot-emacs-package find-dired
    ;;   (setq find-ls-option
    ;;         '("-ls" . "-AGFhlv --group-directories-first --time-style=long-iso"))
    ;;   (setq find-name-arg "-iname"))

    ;; 프롯 커스텀이라고 보면 된다. 검토 필요
    ;; emacs/.emacs.d/prot-lisp/prot-dired.el
    ;; (prot-emacs-package prot-dired
    ;; (:delay 2)
    ;; (add-hook 'dired-mode-hook #'prot-dired-setup-imenu)

    ;; (prot-emacs-keybind dired-mode-map
    ;;                     "i" #'prot-dired-insert-subdir ; override `dired-maybe-insert-subdir'
    ;;                     "/" #'prot-dired-limit-regexp
    ;;                     "C-c C-l" #'prot-dired-limit-regexp
    ;;                     "M-n" #'prot-dired-subdirectory-next
    ;;                     "C-c C-n" #'prot-dired-subdirectory-next
    ;;                     "M-p" #'prot-dired-subdirectory-previous
    ;;                     "C-c C-p" #'prot-dired-subdirectory-previous
    ;;                     "M-s G" #'prot-dired-grep-marked-files))

    ;;;; dired

    ;; (defun jh-workspace/init-dired-hide-dotfiles()
    ;;   ;; Hide hidden files
    ;;   (use-package dired-hide-dotfiles
    ;;     :after dired
    ;;     :defer 2
    ;;     :hook
    ;;     (dired-mode . dired-hide-dotfiles-mode)
    ;;     ;; :bind
    ;;     ;; (:map dired-mode-map ("." . dired-hide-dotfiles-mode))
    ;;     )
    ;;   )

    (defun jh-workspace/init-dired-subtree ()
      (use-package dired-subtree
        :after dired
        :defer 4
        :config
        ;; (setq dired-subtree-use-backgrounds nil)
        (bind-key "<tab>" #'dired-subtree-toggle dired-mode-map)
        )
      )

    ;; ~M-n~ will prompt for strings to narrow the files in current dired buffer.

    ;; dired-mode-map ("M-n" . dired-narrow)
    (defun jh-workspace/init-dired-narrow ()
      (use-package dired-narrow
        :after dired
        :commands dired-narrow
        :defer 5
        :config
        (bind-key "<escape>" #'keyboard-quit dired-narrow-map)
        ))

    (defun jh-workspace/init-dired-filter  ()
      (use-package dired-filter
        :after dired
        :defer 5
        :diminish dired-filter-mode))

    (defun jh-workspace/init-dired-open ()
      (use-package dired-open
        :after dired
        :defer 10
        :commands (dired-open-xdg)
        :config
        (setq dired-open-extensions '(("png" . "feh")
                                      ("mkv" . "mpv")))
        ))

    (defun jh-workspace/init-dired-git-info ()
      (use-package dired-git-info
        :if (not my/slow-ssh)
        :after dired
        :defer 9
        :config
        ;; show git information
        (spacemacs/set-leader-keys-for-major-mode 'dired-mode
          "I" 'dired-git-info-mode)
        (define-key dired-mode-map ")" 'dired-git-info-mode)
        ;; disabled by performance issues
        ;; (add-hook 'dired-after-readin-hook 'dired-git-info-auto-enable)
        ))

    (defun jh-workspace/init-dired-preview ()
      (use-package dired-preview
        :after dired
        :defer 3
        ;; :config
        ;; (add-hook 'dired-mode-hook #'dired-preview-mode)
        )
      )

    ;;;; outli and org-side-tree

    ;;;;; outli

    (defun jh-workspace/init-outli ()
      (use-package outli
        ;; :init
        ;; (setq outli-blend nil)
        :bind (:map outli-mode-map ; convenience key to get back to containing heading
                    ;; ("C-c g" . (lambda () (interactive) (outline-back-to-heading)))
                    ("C-c C-n" . 'outline-next-heading)
                    ("C-c C-p" . 'outline-previous-heading))
        :config
        ;; (add-to-list 'outli-heading-config '(tex-mode "%%" ?% t))
        (add-to-list 'outli-heading-config '(js2-mode "//" ?\/ t))
        (add-to-list 'outli-heading-config '(js-ts-mode "//" ?\/ t))
        (add-to-list 'outli-heading-config '(typescript-mode "//" ?\/ t))
        (add-to-list 'outli-heading-config '(typescript-ts-mode "//" ?\/ t))
        (add-to-list 'outli-heading-config '(python-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(python-ts-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(awk-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(awk-ts-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(elixir-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(elixir-ts-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(sh-mode "##" ?# t))
        (add-to-list 'outli-heading-config '(bash-ts-mode "##" ?# t))

        ;; ess

        (add-to-list 'outli-heading-config '(clojure-mode ";;" ?\; t))
        (add-to-list 'outli-heading-config '(clojurescript-mode ";;" ?\; t))

        (add-hook 'prog-mode-hook 'outli-mode) ; not markdown-mode!
        (add-hook 'org-mode-hook 'outli-mode)
        )
      )

    ;;;;; DONT org-side-tree

    ;; [2023-11-13 Mon 13:34] 충분치 않다.

    ;; (defun jh-workspace/init-org-side-tree ()
    ;;   (use-package org-side-tree
    ;;     :after outli
    ;;     :init
    ;;     (setq outline-minor-mode-cycle t) ; default nil
    ;;     (setq org-side-tree-fontify t)
    ;;     (setq org-side-tree-timer-delay 1.0)
    ;;     )
    ;;   )

    ;;;; Jump between points

    ;;;;; binky

    ;; This package provides commands to jump between points in buffers and files.
    ;; Marked position, last jump position and recent buffers are all supported in
    ;; same mechanism like `point-to-register' and `register-to-point' but with an
    ;; enhanced experience.

    (defun jh-workspace/init-binky ()
      (use-package binky
        :ensure t
        :init
        (setq binky-margin-side 'left)
        ;; flycheck, different margin side
        ;; (setq flycheck-indication-mode 'right-margin)
        :config
        (binky-mode)
        (binky-margin-mode)
        ))

    ;;; Workspace
    ;;;; ibuffer

    (defun jh-workspace/init-nerd-icons-ibuffer ()
      (use-package nerd-icons-ibuffer
        :after nerd-icons
        :if window-system
        :ensure t
        :hook (ibuffer-mode . nerd-icons-ibuffer-mode))
      )

    ;;;; tabspaces

    ;; [[https://github.com/mclear-tools/tabspaces][GitHub - mclear-tools/tabspaces]]
    ;; 탭을 이용한 버퍼 관리 한번 사용해 보자.

    ;; Tabspaces leverages tab-bar.el and project.el (both built into emacs 27+) to
    ;; create buffer-isolated workspaces (or “tabspaces”) that also integrate with
    ;; your version-controlled projects. It should work with emacs 27+.

    ;; While other great packages exist for managing workspaces, such as =bufler=,
    ;; perspective and persp-mode, this package is much less complex, and works
    ;; entirely based on the built-in (to emacs 27+) tab-bar and project packages.

    (defun jh-workspace/init-tabspaces ()
      (use-package tabspaces
        :ensure t
        :after consult
        :commands (tabspaces-switch-or-create-workspace
                   tabspaces-open-or-create-project-and-workspace)
        :custom
        (tabspaces-use-filtered-buffers-as-default t) ; remap
        (tabspaces-default-tab "Default")
        (tabspaces-remove-to-default t)
        (tabspaces-include-buffers '("*scratch*"))
        (tabspaces-use-filtered-buffers-as-default t)

        (tabspaces-initialize-project-with-todo t)
        (tabspaces-todo-file-name "project-todo.org")

        ;; sessions
        ;; (tabspaces-session t)
        ;; (tabspaces-session-auto-restore t)
        :config
        (setq tab-bar-new-tab-choice "*scratch*")
        ;; Filter Buffers for Consult-Buffer
        (with-eval-after-load 'consult
          ;; hide full buffer list (still available with "b" prefix)
          (consult-customize consult--source-buffer :hidden t :default nil)
          ;; set consult-workspace buffer list
          (defvar consult--source-workspace
            (list :name     "*Tabspaces* Buffers"
                  :narrow   ?w
                  :history  'buffer-name-history
                  :category 'buffer
                  :state    #'consult--buffer-state
                  :default  t
                  :items    (lambda () (consult--buffer-query
                                        :predicate #'tabspaces--local-buffer-p
                                        :sort 'visibility
                                        :as #'buffer-name)))
            "Set workspace buffer list for consult-buffer.")
          (add-to-list 'consult-buffer-sources 'consult--source-workspace))
        ) ;; end of tabspace
      )

    ;;;; tab-bar

    ;;;;; tab-bar init

    (defun jh-workspace/init-tab-bar ()
      (use-package tab-bar
        :ensure t
        :config

        (when (display-graphic-p) ; gui
          ;; https://christiantietze.de/posts/2022/02/emacs-tab-bar-numbered-tabs/
          (defvar ct/circle-numbers-alist
            '((0 . "⓪")
              (1 . "①")
              (2 . "②")
              (3 . "③")
              (4 . "④")
              (5 . "⑤")
              (6 . "⑥")
              (7 . "⑦")
              (8 . "⑧")
              (9 . "⑨"))
            "Alist of integers to strings of circled unicode numbers.")
          (defun ct/tab-bar-tab-name-format-default (tab i)
            (let ((current-p (eq (car tab) 'current-tab))
                  (tab-num (if (and tab-bar-tab-hints (< i 10))
                               (alist-get i ct/circle-numbers-alist) "")))
              (propertize
               (concat tab-num
                       " "
                       (alist-get 'name tab)
                       (or (and tab-bar-close-button-show
                                (not (eq tab-bar-close-button-show
                                         (if current-p 'non-selected 'selected)))
                                tab-bar-close-button)
                           "")
                       " ")
               'face (funcall tab-bar-tab-face-function tab))))
          (setq tab-bar-tab-name-format-function #'ct/tab-bar-tab-name-format-default)
          ) ; end-of display-graphic-p

        ;; Tabs for window layouts (tab-bar.el and prot-tab.el)
        ;; =C-M-<number>

        (unless (display-graphic-p) ; terminal
          (setq auto-resize-tab-bars nil) ; important
          (setq tab-bar-separator nil) ; important
          )

        (setq tab-bar-select-tab-modifiers '(control meta))
        (setq tab-bar-new-tab-choice "*scratch*")
        (setq tab-bar-close-button-show nil)
        (setq tab-bar-close-last-tab-choice nil)
        (setq tab-bar-close-tab-select 'recent)
        (setq tab-bar-new-tab-to 'right)
        (setq tab-bar-position nil)
        (setq tab-bar-show nil)
        (setq tab-bar-tab-hints t) ; for tab-bar-circle-number
        ;; (setq tab-bar-auto-width-max '(220 20)) ; Emacs 29

        (setq tab-bar-tab-name-function 'tab-bar-tab-name-current) ; default

        ;; 2023-06-21 TODO customize tab-bar name
        ;; (setq tab-bar-tab-name-function #'+tab-bar-name-fn)
        ;; ;;;###autoload
        ;; (defun +tab-bar-name-fn ()
        ;;   (let* ((project-name (projectile-project-name))
        ;;          (buf-fname (buffer-file-name))
        ;;          (buf-name (buffer-name))
        ;;          (buf-dir (when buf-fname (file-name-directory buf-fname))))
        ;;     (cond
        ;;      ((member major-mode '(gh-notify-mode)) buf-name)

        ;;      ;; project.el
        ;;      ((and project-name
        ;;            (not (string-equal "-" project-name)))
        ;;       project-name)

        ;;      ((eq 'dired-mode major-mode)
        ;;       (projectile-project-name (projectile-project-root default-directory)))

        ;;      ((and buf-dir (projectile-project-p buf-dir))
        ;;       (projectile-project-name (projectile-project-root buf-dir)))

        ;;      (buf-dir buf-dir)

        ;;      ((not (string-match-p "\\*Minibuf" buf-name))
        ;;       buf-name)))
        ;;   )

        (setq tab-bar-format                    ; Emacs 28
              '(
                tab-bar-separator
                tab-bar-format-menu-bar
                tab-bar-format-tabs-groups
                tab-bar-separator
                ;; tab-bar-format-add-tab ;; turn on tab-bar-new-button

                tab-bar-format-align-right
                tab-bar-format-global
                ))

        ;; C-x t 에 바인딩 되어 있다.
        ;; (defun tab-bar-switch-to-tab@override (name)
        ;;   "Like `tab-bar-switch-to-tab', but allow for the creation of a new, named tab on the fly."
        ;;   (interactive
        ;;    (let* ((recent-tabs (mapcar (lambda (tab)
        ;;                                  (alist-get 'name tab))
        ;;                                (tab-bar--tabs-recent))))
        ;;      (list (completing-read (format-prompt "Switch to tab by name"
        ;;                                            (car recent-tabs))
        ;;                             recent-tabs nil nil nil nil recent-tabs))))
        ;;   (if-let ((tab-number (tab-bar--tab-index-by-name name)))
        ;;       (tab-bar-select-tab (1+ tab-number))
        ;;     (tab-bar-new-tab)
        ;;     (tab-bar-rename-tab name)))
        ;; (advice-add #'tab-bar-switch-to-tab :override #'tab-bar-switch-to-tab@override)

        ;; (defun my/project-open-in-tab (project)
        ;;   (interactive (list (project-prompt-project-dir)))
        ;;   (if-let ((tab-number (tab-bar--tab-index-by-name
        ;;                         (file-name-nondirectory (directory-file-name project)))))
        ;;       (tab-bar-select-tab (1+ tab-number))
        ;;     (tab-bar-new-tab)
        ;;     (project-switch-project project)
        ;;     (tab-bar-rename-tab (file-name-nondirectory (directory-file-name project)))))

        (defun my/reload-tab-bar ()
          (interactive)

          (tab-bar-history-mode -1)
          (setq tab-bar-show nil)
          (tab-bar-mode -1)

          (tabspaces-mode t)

          (setq tab-bar-show t)
          (tab-bar-history-mode 1)

          (tab-bar-mode 1))

        ;; explicitly re-enable the cat for the first GUI client
        ;; 순서 상으로 먼저 탭바를 로드하고 테마를 로딩하는게 맞다.
        (add-hook 'after-init-hook #'my/reload-tab-bar)
        ) ; end-of use-package
      ) ; end-of tab-bar

    ;;;;; persp-mode for tab-bar

    ;; https://github.com/Bad-ptr/persp-mode.el/issues/122
    (defun jh-workspace/post-init-persp-mode ()
      (require 'tab-bar)

      ;; Tab Bar maintains window layouts (with optional names). In this, it is
      ;; similar to Perspective. Unlike Perspective, it does not support buffer
      ;; lists. Using Perspective and Tab Bar at the same time is not recommended at
      ;; this time, since the tab list is global (i.e., will show up in all
      ;; perspectives) and is likely to cause confusion. It would be an interesting
      ;; future feature for ?Perspective to adopt the tab bar and allow keeping a
      ;; distinct set of tabs per-perspective.

      (add-hook
       'persp-before-deactivate-functions
       (defun +workspaces-save-tab-bar-data-h (_)
         (when (get-current-persp)
           (set-persp-parameter
            'tab-bar-tabs (tab-bar-tabs)))))

      (add-hook
       'persp-activated-functions
       (defun +workspaces-load-tab-bar-data-h (_)
         (tab-bar-tabs-set (persp-parameter 'tab-bar-tabs))
         (tab-bar--update-tab-bar-lines t)))

      )

    ;; The snippet saves the configuration of tab-bar to files:
    ;; (add-hook
    ;;  'persp-before-save-state-to-file-functions
    ;;  (defun +workspaces-save-tab-bar-data-to-file-h (&rest _)
    ;;    (when (get-current-persp)
    ;;      (set-persp-parameter 'tab-bar-tabs (frameset-filter-tabs (tab-bar-tabs) nil nil t))
    ;;      )
    ;;    )

    ;;;; neotree

    (defun jh-workspace/init-neotree ()
      (use-package neotree
        :ensure t
        :commands neo-global--window-exists-p
        :init
        (setq neo-window-width 32
              neo-create-file-auto-open t
              neo-banner-message "Press ? for neotree help"
              neo-show-updir-line nil
              neo-mode-line-type 'neotree
              neo-smart-open t
              neo-dont-be-alone t
              neo-persist-show nil
              neo-show-hidden-files t
              neo-auto-indent-point t
              neo-modern-sidebar t
              neo-vc-integration nil)

        (when (eq 'darwin system-type)
          (setq neo-default-system-application "open"))

        (spacemacs|define-transient-state neotree
          :title "NeoTree Key Hints"
          :doc "
    Navigation^^^^             Actions^^         Visual actions/config^^^
    ───────^^^^─────────────── ───────^^──────── ───────^^^────────────────
    [_L_]   next sibling^^     [_c_] create      [_TAB_] shrink/enlarge
    [_H_]   previous sibling^^ [_C_] copy        [_|_]   vertical split
    [_J_]   goto child^^       [_d_] delete      [_-_]   horizontal split
    [_K_]   goto parent^^      [_r_] rename      [_gr_]  refresh^
    [_l_]   open/expand^^      [_R_] change root [_s_]   hidden:^^^ %s(if neo-buffer--show-hidden-file-p \"on\" \"off\")
    [_h_]   up/collapse^^      ^^                ^^^
    [_j_]   line down^^        ^^                ^^^
    [_k_]   line up^^          ^^                ^^
    [_'_]   quick look         ^^                ^^
    [_RET_] open               ^^^^              [_?_]   close hints
    "
          :bindings
          ("RET" spacemacs/neotree-expand-or-open)
          ("TAB" neotree-stretch-toggle)
          ("|" neotree-enter-vertical-split)
          ("-" neotree-enter-horizontal-split)
          ("?" nil :exit t)
          ("'" neotree-quick-look)
          ("c" neotree-create-node)
          ("C" neotree-copy-node)
          ("d" neotree-delete-node)
          ("gr" neotree-refresh)
          ("h" spacemacs/neotree-collapse-or-up)
          ("H" neotree-select-previous-sibling-node)
          ("j" neotree-next-line)
          ("J" neotree-select-down-node)
          ("k" neotree-previous-line)
          ("K" neotree-select-up-node)
          ("l" spacemacs/neotree-expand-or-open)
          ("L" neotree-select-next-sibling-node)
          ("r" neotree-rename-node)
          ("R" neotree-change-root)
          ("s" neotree-hidden-file-toggle))

        (defun spacemacs//neotree-key-bindings ()
          "Set the key bindings for a neotree buffer."
          (evilified-state-evilify-map neotree-mode-map
            :mode neotree-mode
            :bindings
            (kbd "TAB") 'neotree-stretch-toggle
            (kbd "RET") 'spacemacs/neotree-expand-or-open
            (kbd "|") 'neotree-enter-vertical-split
            (kbd "-") 'neotree-enter-horizontal-split
            (kbd "'") 'neotree-quick-look
            (kbd "c") 'neotree-create-node
            (kbd "C") 'neotree-copy-node
            (kbd "d") 'neotree-delete-node
            (kbd "gr") 'neotree-refresh
            (kbd "h") 'spacemacs/neotree-collapse-or-up
            (kbd "H") 'neotree-select-previous-sibling-node
            (kbd "j") 'neotree-next-line
            (kbd "J") 'neotree-select-down-node
            (kbd "k") 'neotree-previous-line
            (kbd "K") 'neotree-select-up-node
            (kbd "l") 'spacemacs/neotree-expand-or-open
            (kbd "L") 'neotree-select-next-sibling-node
            (kbd "q") 'neotree-hide
            (kbd "r") 'neotree-rename-node
            (kbd "R") 'neotree-change-root
            (kbd "zz") 'evil-scroll-line-to-center
            (kbd "zt") 'evil-scroll-line-to-top
            (kbd "zb") 'evil-scroll-line-to-bottom
            (kbd "?") 'spacemacs/neotree-transient-state/body
            (kbd "s") 'neotree-hidden-file-toggle))

        ;; conflict with treemacs
        ;; (spacemacs/set-leader-keys
        ;;   "ft" 'neotree-toggle
        ;;   "fT" 'neotree-show
        ;;   "pt" 'neotree-find-project-root)
        :config
        (spacemacs//neotree-key-bindings)
        (add-to-list 'spacemacs-window-split-ignore-prefixes neo-buffer-name)
        (add-to-list 'spacemacs-window-split-ignore-prefixes imenu-list-buffer-name)
        )
      )

    ;;; Project

    ;;;; projectile

    (defun jh-workspace/post-init-projectile ()
      ;; Projectile on Emacs with REMOTE Access
      ;; /vanilla/mpereira-dotfiles-evil-clojure/configuration.org
      (setq projectile-enable-caching t)
      (setq projectile-require-project-root t)
      (setq projectile-dynamic-mode-line nil)

      ;; Prevent projectile from automatically creating projects when visiting
      ;; files. For example, navigating to the definition of a function from a
      ;; dependency will add the dependency directory as a project.

      ;; 파일을 방문할 때 프로젝트가 자동으로 프로젝트를 생성하지 않도록 합니다.
      ;; 예를 들어 종속성에서 함수 정의로 이동하면 종속성 디렉터리가 프로젝트로
      ;; 추가됩니다.
      (setq projectile-track-known-projects-automatically nil)

      ;; TODO work with TRAMP
      ;; (defun mpereira/projectile-default-project-name (project-root)
      ;;   "TODO: PROJECT-ROOT docstring."
      ;;   (let* ((default-directory project-root)
      ;;          (suffix (if (file-remote-p project-root)
      ;;                      (format " @ %s" (mpereira/remote-host))
      ;;                    "")))
      ;;     (concat (file-name-nondirectory (directory-file-name default-directory))
      ;;             suffix)))

      ;; (setq projectile-project-name-function 'mpereira/projectile-default-project-name)

      ;; (setq projectile-switch-project-action 'project-dired)
      )

    ;;;; DONT git-commit

    ;; Enforce git commit conventions.
    ;; See: chris.beams.io/posts/git-commit/
    ;; Use Spacemacs as the $EDITOR
    ;; (or $GIT_EDITOR) for git commits messages when using git commit on the command

    ;; 프로젝트 prefix p
    ;; f find :: projectile-find-file
    ;; p switch project :: projectile-switch-project
    ;; k kill buffers :: projectile-kill-buffers
    ;; s search in project :: counsel-git-grep ==> consult-git-grep
    ;; S consult-git-log-grep
    ;; b switch to project buffer :: projectile-switch-to-buffer

    ;;;; consult-git-log-grep

    (defun jh-workspace/init-consult-git-log-grep ()
      (use-package consult-git-log-grep :defer 7))

    ;;;; consult-projectile

    (defun jh-workspace/init-consult-projectile ()
      (use-package consult-projectile
        ;; :if (not (or my/remote-server *is-termux*))
        ;; package provides a function I use everyday: ~M-x consult-projectile~.  When
        ;; I invoke ~consult-projectile~, I have the file completion for the current
        ;; project.  I can also type =b= + =SPACE= to narrow my initial search to open
        ;; buffers in the project.  Or =p= + =space= to narrow to other projects; and
        ;; then select a file within that project.
        ;; :after consult ; projectile
        :defer 2
        :commands (consult-projectile my/consult-buffer)
        :config
        (consult-customize
         consult-projectile
         consult-projectile-recentf
         consult-projectile-find-dir
         consult-projectile-find-file
         consult-projectile-switch-to-buffer
         consult-projectile-switch-project
         :preview-key '("M-." "C-SPC"
                        :debounce 0.3 "<up>" "<down>" "C-n" "C-p"
                        ))

        (setq consult-projectile--source-projectile-buffer
              (list :name     "Projectile Buffer"
                    :narrow   '(?j . "Projectile Buffer")
                    :category 'buffer
                    :face     'consult-buffer
                    :history  'buffer-name-history
                    :state    #'consult--buffer-state
                    :enabled  #'projectile-project-root
                    :items
                    (lambda ()
                      (when-let (root (projectile-project-root))
                        (mapcar #'buffer-name
                                (seq-filter (lambda (x)
                                              (when-let (file (buffer-file-name x))
                                                (string-prefix-p root file)))
                                            (consult--buffer-query :sort 'visibility)))))))

        (setq consult-projectile-sources
              '(consult-projectile--source-projectile-buffer
                consult-projectile--source-projectile-file
                consult-projectile--source-projectile-project
                consult-projectile--source-projectile-recentf
                ))
        )
      )

    ;;;; DONT magit-imerge

    ;; (defun jh-workspace/init-magit-imerge ()
    ;;   (use-package magit-imerge
    ;;     :if (not (or my/remote-server *is-termux*))
    ;;     :after magit
    ;;     :defer t)
    ;;   )

    ;;;; blamer

    (defun jh-workspace/init-blamer ()
      (use-package blamer
        :if (not (or my/remote-server *is-termux*))
        ;; :bind (("M-s-i" . blamer-show-commit-info)
        ;;         ("M-s-n" . blamer-mode))
        :defer 7
        :custom
        ;; Set to 0 because I don’t enable by default.  So I’m in a mindset of show
        ;; me who and when.
        (blamer-show-avatar-p nil)
        (blamer-idle-time 0.0)
        (blamer-author-formatter "✎ %s ")
        (blamer-datetime-formatter "[%s] ")
        (blamer-commit-formatter "● %s")
        (blamer-min-offset 40)
        (blamer-max-commit-message-length 20)
        :custom-face
        (blamer-face ((t :foreground "#9099AB"
                         :background unspecified
                         :height .9
                         :italic t))))
      )

    ;;;; git-cliff

    ;; This package provides the interface of `git-cliff`, built in transient, to
    ;; generate and update changelog for project.  Call `git-cliff-menu` to start.
    (defun jh-workspace/init-git-cliff ()
      (use-package git-cliff
        :after magit transient
        :defer 8
        :custom
        (git-cliff-enable-examples t)
        :config
        ;; git-cliff-extra-path : directory storing user defined presets and templates.
        ;; Integrate to `magit-tag'
        (with-eval-after-load 'magit-tag
          (transient-append-suffix 'magit-tag
            '(1 0 -1)
            '("c" "changelog" git-cliff-menu)))
        )
      )


    ;;;; consult-gh : github plugin

    ;; (defun jh-workspace/init-consult-gh ()
    ;;   (use-package consult-gh
    ;;     :custom
    ;;     (consult-gh-repo-maxnum 30) ;;set max number of repos to 30
    ;;     (consult-gh-issues-maxnum 100) ;;set max number of issues to 100
    ;;     ;; (consult-gh-show-preview t)
    ;;     ;; (consult-gh-preview-key "M-.")
    ;;     (consult-gh-large-file-warning-threshold 2500000)
    ;;     (consult-gh-prioritize-local-folder 'suggest)
    ;;     (consult-gh-preview-buffer-mode 'org-mode) ;; show previews in org-mode

    ;;     (consult-gh-repo-action #'consult-gh--repo-browse-files-action) ;;open file tree of repo on selection
    ;;     (consult-gh-issue-action #'consult-gh--issue-view-action) ;;open issues in an emacs buffer
    ;;     (consult-gh-pr-action #'consult-gh--pr-view-action) ;;open pull requests in an emacs buffer
    ;;     (consult-gh-code-action #'consult-gh--code-view-action) ;;open files that contain code snippet in an emacs buffer
    ;;     (consult-gh-file-action #'consult-gh--files-view-action) ;;open files in an emacs buffer
    ;;     :config
    ;;     (require 'consult-gh-embark)
    ;;     ;; set the default folder for cloning repositories, By default Consult-GH will confirm this before cloning
    ;;     (setq consult-gh-default-clone-directory "~/code")
    ;;     (setq consult-gh-default-save-directory "~/Downloads")
    ;;     (setq consult-gh-default-orgs-list '("junghan0611" "junghanacs"))
    ;;     ;; (setq consult-gh-default-orgs-list (append consult-gh-default-orgs-list '("alphapapa" "systemcrafters")))
    ;;     )
    ;;   )

    ;;;; DONT side-hustle

    ;; (defun jh-workspace/init-side-hustle ()
    ;;   (require 'side-hustle)
    ;;   (setq side-hustle-indent-width 2)
    ;;   (global-set-key (kbd "M-g h") 'side-hustle-toggle)
    ;;   )

    ;;; packages.el ends here

    ```


#### <span class="section-num">3.5.3</span> The `jh-workspace` funcs.el {#h:9d1f0f11-9645-441d-8ec0-2575fbb30ee9}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

(defun my/dired-create-empty-file-subtree ()
  (interactive)
  (let ((default-directory (dired-current-directory)))
    (dired-create-empty-file
     (read-file-name "Create empty file: "))))

;;; neotree

;; copy from spacemacs

(defun spacemacs/neotree-smart-focus ()
  "Focus the `neotree' buffer, creating as necessary.
If the imenu-list buffer is displayed in any window, focus it, otherwise create and focus.
Note that all the windows in every frame searched, even invisible ones, not
only those in the selected frame."
  (interactive)
  (if (get-buffer-window neo-buffer-name t)
      (neotree-show)
    (neotree-toggle)))

(defun spacemacs/neotree-expand-or-open (&optional arg)
  "Expand or open a neotree node."
  (interactive "P")
  (let ((node (neo-buffer--get-filename-current-line)))
    (when node
      (if (file-directory-p node)
          (progn
            (neo-buffer--set-expand node t)
            (neo-buffer--refresh t)
            (when neo-auto-indent-point
              (next-line)
              (neo-point-auto-indent)))
        (if arg
            (neotree-enter arg)
          (let ((mru-winum (winum-get-number (get-mru-window))))
            (apply 'neotree-enter (list mru-winum))))))))

(defun spacemacs/neotree-collapse ()
  "Collapse a neotree node."
  (interactive)
  (let ((node (neo-buffer--get-filename-current-line)))
    (when node
      (when (file-directory-p node)
        (neo-buffer--set-expand node nil)
        (neo-buffer--refresh t))
      (when neo-auto-indent-point
        (neo-point-auto-indent)))))

(defun spacemacs/neotree-collapse-or-up ()
  "Collapse an expanded directory node or go to the parent node."
  (interactive)
  (let ((node (neo-buffer--get-filename-current-line)))
    (when node
      (if (file-directory-p node)
          (if (neo-buffer--expanded-node-p node)
              (spacemacs/neotree-collapse)
            (neotree-select-up-node))
        (neotree-select-up-node)))))

(defun neotree-find-project-root ()
  (interactive)
  (if (neo-global--window-exists-p)
      (neotree-hide)
    (let ((origin-buffer-file-name (buffer-file-name)))
      (neotree-find (projectile-project-root))
      (neotree-find origin-buffer-file-name))))

(defun spacemacs//neotree-maybe-attach-window ()
  (when (get-buffer-window (neo-global--get-buffer))
    (neo-global--attach)))

```


#### <span class="section-num">3.5.4</span> The `jh-workspace` keybindings.el {#h:3388819e-750a-4487-a5b5-69f1c6362305}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; Dired

;; (global-set-key (kbd "M-g S") 'dired-sidebar-toggle-sidebar)
;; (spacemacs/set-leader-keys-for-major-mode 'dired-mode "." 'dired-hide-dotfiles-mode)
(spacemacs/set-leader-keys-for-major-mode 'dired-mode "p" 'dired-preview-mode)
(spacemacs/set-leader-keys-for-major-mode 'dired-mode "D" 'dired-du-mode)

;;; evil-jump or better-jumper
(spacemacs/set-leader-keys "j." 'evil-jump-forward)
(spacemacs/set-leader-keys "j," 'evil-jump-backward)

;;; imenu-list / treemacs / neotree

(global-set-key (kbd "<f7>") 'neotree-toggle)
(global-set-key (kbd "<f8>") 'imenu-list-smart-toggle)
(global-set-key (kbd "<f10>") 'spacemacs/treemacs-project-toggle)

;;; binky

(spacemacs/set-leader-keys "j 9" 'binky-binky)

(define-prefix-command 'my-binky-map)
(define-key global-map (kbd "M-g 9") 'my-binky-map)
(let ((map my-binky-map))
  (define-key map (kbd "b") 'binky-binky)
  (define-key map (kbd "v") 'binky-view)
  (define-key map (kbd "a") 'binky-add)
  (define-key map (kbd "j") 'binky-jump)
  (define-key map (kbd "d") 'binky-delete))

;;; dumb-jump
(global-set-key (kbd "M-g j") 'dumb-jump-hydra/body)
(spacemacs/set-leader-keys "j/" 'dumb-jump-hydra/body)

;;; Tab-bar

;; Replace Emacs Tabs key bindings with Workspace key bindings
(with-eval-after-load 'evil-maps
  ;; vim-style tab switching
  (define-key evil-motion-state-map "gt" 'tab-next)
  (define-key evil-motion-state-map "gT" 'tab-previous)
  (define-key evil-normal-state-map "gt" 'tab-next)
  (define-key evil-normal-state-map "gT" 'tab-previous)
  )

;;; Magit

(define-prefix-command 'my-magit-map)
(define-key global-map (kbd "C-x g") 'my-magit-map)
(let ((map my-magit-map))
  (define-key map (kbd "m") 'magit-status)
  (define-key map (kbd "f") 'magit-file-dispatch)
  (define-key map (kbd "d") 'magit-dispatch)
  (define-key map (kbd "l") 'git-link)
  ;; (define-key map (kbd "g") 'consult-gh-search-repos)
  ;; (define-key map (kbd "/") 'git-cliff-menu)
  )

;; SPC g l l 'git-link
(global-set-key (kbd "M-g l") 'git-link)

;;; consult
(spacemacs/set-leader-keys "gg" 'consult-git-grep)
(spacemacs/set-leader-keys "gG" 'consult-git-log-grep)

(spacemacs/set-leader-keys "bj" 'consult-projectile)
(global-set-key (kbd "M-s j") 'consult-projectile)

;;; burly

;; M-m 누르면 Spacemacs Root 키바인딩 -- b I 누르면 된다. 이게 더 분류상 편하다.
(spacemacs/declare-prefix "bM"  "burly-bookmark")
(spacemacs/set-leader-keys "bMw" 'burly-bookmark-windows)
(spacemacs/set-leader-keys "bMo" 'burly-open-bookmark)
(spacemacs/set-leader-keys "bMl" 'burly-open-last-bookmark)

;;; keybindings.el End

```


### <span class="section-num">3.6</span> The `jh-checker` layer {#h:4c0ff9b3-e222-4c43-b485-b53cd4f5524a}




#### <span class="section-num">3.6.1</span> The `jh-checker` layer.el {#h:06f24b3b-9eed-441e-aa1f-e1db09685023}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   ;; Spell as you type with Flyspell package,
   ;; requires external command - ispell, hunspell, aspell
   ;; SPC S menu, SPC S s to check current word
   (spell-checking
    ;; flyspell flyspell-correct
    :packages (not auto-dictionary flyspell-popup flyspell-correct-popup)
    :variables
    enable-flyspell-auto-completion nil
    spell-checking-enable-auto-dictionary nil
    spell-checking-enable-by-default nil)

   ;; flycheck :: keep flycheck-posframe (x) -- terminal issue
   (syntax-checking
    :packages (not popwin flycheck-pos-tip)
    :variables
    syntax-checking-enable-by-default nil ; default t - prog-mode
    ;; syntax-checking-enable-tooltip nil ; default t
    ;; syntax-checking-auto-hide-tooltips 3 ;
    ;; syntax-checking-use-standard-error-navigation t ; default nil
    )
   )
 )

```


#### <span class="section-num">3.6.2</span> The `jh-checker` packages.el {#h:ade7815f-9d90-42e0-bf8e-393939556476}

<!--list-separator-->

1.  Packages



    ```elisp

    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-checker-packages
      '(
        flymake
        flycheck

    ;;;;; 1) Syntax
        consult-flycheck
        consult-flyspell

    ;;;;; 2) Spell
        flyspell
        (jinx :location (recipe :fetcher github :repo "minad/jinx"
                                :files ("*.*")))

        ;; wcheck-mode ; with enchant2 for english

    ;;;;; 3) Style
        ;; 1. linters
        ;; flycheck-grammarly
        ;; flycheck-vale

        ;; 2. languagetool
        ;; languagetool
        ;; flycheck-languagetool
        ;; lsp-ltex
        ;; (lsp-ltex :location (recipe :fetcher github :repo "emacs-languagetool/lsp-ltex"))

        ;; 3. grammarly
        ))

    ```

<!--list-separator-->

2.  Configurations

    ```elisp
    ;;;; Syntax checker

    ;;;;; flycheck

    (defun jh-checker/post-init-flycheck ()
      ;; Default value is (save idle-change new-line mode-enabled)
      ;; (setq flycheck-check-syntax-automatically
      ;;       '(save idle-buffer-switch mode-enabled))

      ;; use 'M-g f' consult-flycheck
      ;; (evil-define-key '(normal) flycheck-mode-map (kbd "M-n") 'flycheck-next-error)
      ;; (evil-define-key '(normal) flycheck-mode-map (kbd "M-p") 'flycheck-previous-error)

      ;;/ahyatt-dotfiles/.emacs.d/init.el:569
      ;; (setq flycheck-disabled-checkers '(emacs-lisp-checkdoc))

      ;; Wait before complaining so we don't step on useful help messages.
      (setq-default flycheck-idle-change-delay 1.0) ; default 0.5
      )

    (defun jh-checker/init-consult-flycheck ()
      (use-package consult-flycheck
        :after flycheck
        :defer 6
        :bind (:map flycheck-command-map
                    ("!" . consult-flycheck))
        ;; If flycheck idle change delay is too short, then it overwrites the helpful
        ;; messages about how to call elisp functions, etc.
        :config
        (global-set-key (kbd "M-g F") 'consult-flycheck)
        )
      )

    ;;;;; flymake

    (defun jh-checker/init-flymake ()
      (use-package flymake
        :ensure t
        :bind (:map flymake-mode-map
                    ("M-p" . flymake-goto-prev-error)
                    ("M-n" . flymake-goto-next-error)
                    ;; ("M-g D"   . flymake-show-buffer-diagnostics)
                    ;; ("M-g P" . flymake-show-project-diagnostics)
                    :repeat-map flymake-repeatmap
                    ("p" . flymake-goto-prev-error)
                    ("n" . flymake-goto-next-error)
                    :map flymake-diagnostics-buffer-mode-map
                    ("?" . flymake-show-diagnostic-here)
                    :map flymake-project-diagnostics-mode-map
                    ("?" . flymake-show-diagnostic-here))
        :init
        ;; left - flycheck , right - flymake and git-gutter-fringe
        ;; (setq flymake-fringe-indicator-position 'right-fringe)
        ;; (evil-define-key '(normal) flymake-mode-map (kbd "M-n") 'flymake-goto-next-error)
        ;; (evil-define-key '(normal) flymake-mode-map (kbd "M-p") 'flymake-goto-prev-error)
        (defun flymake-show-diagnostic-here (pos &optional other-window)
          "Show the full diagnostic of this error.
    Used to see multiline flymake errors"
          (interactive (list (point) t))
          (let* ((id (or (tabulated-list-get-id pos)
                         (user-error "Nothing at point")))
                 (text (flymake-diagnostic-text (plist-get id :diagnostic))))
            (message text)))
        (remove-hook 'flymake-diagnostic-functions #'flymake-proc-legacy-flymake))
      )

    ;;;; Spell checker

    ;;;;; flyspell for korean

    (defun jh-checker/post-init-flyspell ()
      (require 'ispell)
      (setq-default ispell-program-name "hunspell")
      (setq ispell-really-hunspell t)

      ;; 김아더 WordNet -- 테스트 완료
      (add-to-list 'ispell-skip-region-alist '(":\\(PROPERTIES\\|LOGBOOK\\):" . ":END:"))
      (add-to-list 'ispell-skip-region-alist '("#\\+BEGIN_SRC" . "#\\+END_SRC"))
      (add-to-list 'ispell-skip-region-alist '("#\\+BEGIN_EXAMPLE" . "#\\+END_EXAMPLE"))

      ;; −B Report run-together words with missing blanks as spelling errors.
      ;; −C Consider run-together words as valid compounds.
      (setq ispell-local-dictionary-alist
            '(
              ;; ("english_american"
              ;;  "[A-Za-z]" "[^A-Za-z]" "[0-9a-zA-Z]" nil
              ;;  ("-d" "en_US")
              ;;  nil utf-8)
              ("ko"
               "[가-힣]" "[^가-힣]"
               "[0-9a-zA-Z]" nil
               ("-d" "ko_KR")
               nil utf-8)
              ;; ("korean,english_american"
              ;;  "[가-힣 A-Za-z]" "[^가-힣 A-Za-z]" "[0-9a-zA-Z]" nil
              ;;  ("-d" "ko_KR,en_US")
              ;;  nil utf-8)
              ;; ("korean-english"
              ;;  "[가-힣 A-Za-z]" "[^가-힣 A-Za-z]" "[0-9a-zA-Z]" nil
              ;;  ("-d" "ko_KR2")
              ;;  nil utf-8)
              ))

      ;; 수동으로 설정한다.
      (setq ispell-hunspell-dictionary-alist ispell-local-dictionary-alist)
      (setq flyspell-default-dictionary "ko")

      ;; (add-to-list 'ispell-hunspell-dict-paths-alist '("en_US" "/Users/jay/Library/Spelling/en_US.aff"))
      ;; (setq ispell-hunspell-dict-paths-alist '(("ko" "/usr/share/hunspell/ko_KR.aff")))

      ;; 위 에 수동으로 잡아주면 아래 할 필요 없다. 수동으로 하는게 더 정확하다.
      ;; ispell-set-spellchecker-params has to be called
      ;; before ispell-hunspell-add-multi-dic will work
      ;; (ispell-set-spellchecker-params)
      ;; (ispell-hunspell-add-multi-dic "english_american,korean")
      (setq ispell-dictionary "ko")
      ;; (setq ispell-local-dictionary "korean,english_american")
      (setq ispell-personal-dictionary "~/.hunspell_personal")

      ;; delay for korean default
      ;; (setq flyspell-delay 1.0)

      ;; ;; korean - english
      ;; (let ((langs '("korean" "english_american")))
      ;;   (setq lang-ring (make-ring (length langs)))
      ;;   (dolist (elem langs) (ring-insert lang-ring elem)))
      ;; (defun cycle-ispell-languages ()
      ;;   (interactive)
      ;;   (let ((lang (ring-ref lang-ring -1)))
      ;;     (ring-insert lang-ring lang)
      ;;     (ispell-change-dictionary lang)))
      ;; key bindings
      ;; (global-set-key (kbd "C-S-SPC") 'cycle-ispell-languages)

      ;; see : https://whhone.com/posts/write-with-emacs-1/
      ;; (add-hook 'text-mode-hook 'flyspell-mode)

      ;; (when (display-graphic-p) ; gui
      ;;   (add-hook 'org-mode-hook 'flyspell-mode)
      ;;   )
      ;; (add-hook 'prog-mode-hook 'flyspell-prog-mode) ; only check comments

      ;; C-,    flyspell-goto-next-error
      ;; C-.    flyspell-auto-correct-word
      ;; C-;    flyspell-correct-wrapper
      ;; C-M-i  flyspell-auto-correct-word -> nil
      ;; C-c $  flyspell-correct-word-before-point

      ;; =flyspell-correct-completing-read interface=
      ;; In order to select the (s)ave, (a)ccept, sto(p) or s(k)ip actions,
      ;;  you can enter the shortcut key @s, @a, @p or @k. The actions
      ;;  will be automatically submitted. Furthermore suggestions can be
      ;;  quickly submitted by entering the numeric index in front of the
      ;;  suggestion. Besides the quick keys the usual completing-read
      ;;  interface is available, which may be enhanced and allow
      ;;  scrolling through candidates if you have a completion UI like
      ;;  Icomplete-vertical, Selectrum or Vertico installed.
      (require 'flyspell-correct)
      (define-key flyspell-mode-map (kbd "C-;") 'flyspell-correct-wrapper)
      (define-key flyspell-mode-map (kbd "C-M-i") nil) ;  for completion-at-point
      ;; evil key-bindings with [, ]
      (define-key ctl-x-x-map "s" #'flyspell-mode) ; C-x x s
      )

    ;;;;; jinx for English

    (defun jh-checker/init-jinx ()
      (use-package jinx
        :if (not (or my/remote-server *is-termux*))
        :defer 5
        :init
        (spacemacs/declare-prefix "S"  "spelling")
        (spacemacs/set-leader-keys "Sj" 'jinx-correct)
        (spacemacs/set-leader-keys "SJ" 'jinx-mode)
        :config
        (setq jinx-delay 1.0) ; default 0.2

        ;; (dolist (hook '(text-mode-hook conf-mode-hook)) ; prog-mode-hook
        ;;   (add-hook hook #'jinx-mode))
        ;; (add-hook 'prog-mode-hook #'jinx-mode) ; 주석

        ;; 한글 제외 : 영어 검사
        (setenv "LANG" "en_US.UTF-8")
        (setq jinx-languages "en")
        (add-hook 'jinx-mode-hook (lambda () (setq-local jinx-languages "en")))
        (setq jinx-exclude-regexps
              '((emacs-lisp-mode "Package-Requires:.*$")
                (t "[가-힣]" "[A-Z]+\\>" "-+\\>" "\\w*?[0-9]\\w*\\>" "[a-z]+://\\S-+" "<?[-+_.~a-zA-Z][-+_.~:a-zA-Z0-9]*@[-.a-zA-Z0-9]+>?" "\\(?:Local Variables\\|End\\):\\s-*$" "jinx-\\(?:languages\\|local-words\\):\\s-+.*$")))

        ;; 1) 개인사전 연결해줄 것
        ;; ~/.config/enchant/en.dic -> /home/junghan/.aspell_en_personal
        ;; ko.dic -> /home/junghan/.aspell_en_personal 뭐지? 이렇게 들어가네?!
        ;; 되는데로 하자
        ;; 2) enchant.ordering aspell 로 변경할 것
        ;; 	*:nuspell,aspell,hunspell
        ;; en_AU:aspell,hunspell,nuspell
        ;; en_CA:aspell,hunspell,nuspell
        ;; en_GB:aspell,hunspell,nuspell
        ;; en_US:aspell,hunspell,nuspell
        ;; en:aspell,hunspell,nuspell

        ;; 영어 제외 : 한글만 검사
        ;; (setq jinx-languages "ko")
        ;; (setq jinx-exclude-regexps
        ;;       '((t "[A-Za-z]" "[']")))

        ;; 아래는 기본인데 일단 해보면서 보자.
        ;; "[A-Z]+\\>"         ;; Uppercase words
        ;; "\\w*?[0-9]\\w*\\>" ;; Words with numbers, hex codes
        ;; "[a-z]+://\\S-+"    ;; URI
        ;; "<?[-+_.~a-zA-Z][-+_.~:a-zA-Z0-9]*@[-.a-zA-Z0-9]+>?" ;; Email
        ;; "\\(?:Local Variables\\|End\\):\\s-*$" ;; Local variable indicator
        ;; "jinx-\\(?:languages\\|local-words\\):\\s-+.*$")) ;; Local variables

        ;; M-$점 앞의 철자가 틀린 단어에 대한 수정을 트리거합니다.
        ;; C-u M-$전체 버퍼에 대한 수정을 트리거합니다.
        (keymap-global-set "M-$" #'jinx-correct)
        ;; (keymap-global-set "C-;" #'jinx-correct)
        (keymap-global-set "C-M-$" #'jinx-languages)
        ;; (keymap-global-set "<remap> <ispell-word>" #'jinx-correct)
        ;; (define-key jinx-misspelled-map (kbd "C-;") 'jinx-correct)
        ))

    ;;;;; consult-flyspell

    (defun jh-checker/init-consult-flyspell ()
      (use-package consult-flyspell :defer 8
        :config
        (setq consult-flyspell-always-check-buffer t)
        (spacemacs/set-leader-keys "Sf" 'consult-flyspell)
        )
      )

    ;;;; TODO Style checker

    ;; Linter
    ```


#### <span class="section-num">3.6.3</span> The `jh-checker` funcs.el {#h:b7eab714-4f62-4aca-a944-f24d00dd0922}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

(defun my/add-word-to-personal-dict-flyspell ()
  (interactive)
  (let ((current-location (point))
        (word (flyspell-get-word)))
    (when (consp word)
      (flyspell-do-correct 'save nil (car word) current-location (cadr word) (caddr word) current-location))))

```


### <span class="section-num">3.7</span> The `jh-writing` layer {#h:13a613b4-a45f-4281-8aa5-36309bac9dc0}




#### <span class="section-num">3.7.1</span> The `jh-writing` layer.el {#h:edc97a60-b9f9-4d09-b0cb-a687768aa57a}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;; spacemacs-editing, editing-visual, evil ...

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(

;;;; Editing

   copy-as-format

   ;; `g r' menu in Emacs normal state
   ;; 2023-11-21 disable evil-mc that conflicts own keybindings C-n/C-p etc.
   (multiple-cursors :variables
                     multiple-cursors-backend 'mc
                     mc/cmds-to-run-once '(upcase-region))

   (xclipboard :variables xclipboard-enable-cliphist t)

   ;; not password-generator drag-stuff undo-tree aggressive-indent editorconfig
   (spacemacs-editing
    :packages
    (dired-quick-sort
     eval-sexp-fu
     ;; smartparens
     spacemacs-whitespace-cleanup
     pcre2el
     string-edit-at-point
     uuidgen
     evil-easymotion
     expand-region
     hexl
     ws-butler
     unkillable-scratch
     hungry-delete
     persistent-scratch
     string-inflection
     avy
     link-hint
     lorem-ipsum
     evil-swap-keys
     ))

   (spacemacs-editing-visual
    :packages
    (hide-comnt
     rainbow-delimiters
     volatile-highlights
     term-cursor
     writeroom-mode ; depend to eww layer
     ))

   ;; 2023-09-03 remove outline
   ;; 2023-11-03 evil-lisp-state evil-cleverparens ; depends on smartparens
   (spacemacs-evil
    :packages
    (evil-anzu
     ;; evil-goggles
     evil-args
     evil-collection
     evil-escape
     evil-exchange
     evil-iedit-state
     evil-indent-plus
     evil-lion
     evil-nerd-commenter
     evil-matchit
     evil-numbers
     evil-surround
     evil-textobj-line
     evil-unimpaired
     evil-visual-mark-mode
     evil-visualstar
     evil-tutor
     eldoc hs-minor-mode)
    :variables
    spacemacs-evil-collection-allowed-list ;; evil-collection-mode-list
    '(dired quickrun ediff eww ;; spacemacs default
            ;; outline wdired wgrep - 넣지 말자. 스페이스맥스 권장 키 이용
            xref eglot flymake tar-mode thread
            tab-bar man dashboard shortdoc info ; speedbar
            devdocs corfu
            youtube-dl rg deadgrep leetcode
            disk-usage
            (image image-mode)
            emms ebuku
            atomic-chrome
            arc-mode
            calc
            bookmark
            calendar
            debug
            dictionary
            ement
            pass
            popup
            proced
            profiler
            (process-menu simple)
            telega
            vlf
            vundo
            woman
            ))


   ;; 이것 때문에 실수로 Replace 하는 경우가 생긴다.
   ;; (evil-snipe :variables evil-snipe-enable-alternate-f-and-t-behaviors t)

   ;; evil-better-jumper

   ;; vim-empty-lines ; Never!

;;;; Writing

   asciidoc ; e.g. docs.cider.mx editing

   ;; mmm-mode가 jit-lock 문제 발생 시켜 삭제
   (markdown
    :packages (markdown-mode markdown-toc gh-md) ; mmm-mode vmd-mode
    ;; :variables markdown-live-preview-engine 'vmd
    ;; markdown-mmm-auto-modes '("c" "c++" "python" "scala" ("elisp" "emacs-lisp"))
    )

   (translate
    :variables gts-translate-list '(("en" "ko") ("ja" "ko") ("ko" "en"))
    translate-reference-buffer-read-only nil
    translate-enable-highlight t
    translate/paragraph-render 'buffer
    translate/word-render 'buffer ;; default 'posframe
    )

   (plantuml :variables
             plantuml-jar-path "/usr/share/plantuml/plantuml.jar"
             org-plantuml-jar-path "/usr/share/plantuml/plantuml.jar")

   ;; (typography :packages (typo)
   ;;             :variables typography-enable-typographic-editing nil)

   ;; fountain ; screenwriting

   ;; restructuredtext ; ReStructuredText (ReST)

   ;; (latex :variables
   ;;        latex-enable-folding t
   ;;        latex-enable-auto-fill t)


   ;; (languagetool :variables
   ;;               langtool-default-language "en-US"
   ;;               langtool-show-error-on-jump t
   ;;               langtool-java-classpath "/usr/share/languagetool:/usr/share/java/languagetool/*")
   )
 )

```


#### <span class="section-num">3.7.2</span> The `jh-writing` packages.el {#h:82110e67-f8e7-4d28-9107-07e0da44914d}

<!--list-separator-->

1.  Packages

    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-writing-packages
      '(
    ;;;;; Editing
        puni ; replace smartparens

        evil
        evil-escape
        evil-visualstar
        evil-lion

        visual-regexp
        rg ; ripgrep tool with wgrep
        affe ; async fuzzy finder
        deadgrep ; ripgrep tool
        find-file-in-project
        unfill
        undo-fu
        pulsar

        pangu-spacing

        phi-search ; with multiple-cursors

    ;;;;; add more evil pkgs from injae-dotfiles and etc

        evil-goggles
        evil-string-inflection
        ;; evil-traces
        ;; evil-owl
        ;; move-text

    ;;;;; Writing

        visual-fill-column

        binder
        dictionary
        markdown-mode

        (guess-language :location (recipe
                                   :fetcher github
                                   :repo "junghan0611/guess-language.el"
                                   :files ("*.el" "trigrams/*")))

        ;; (txl :location (recipe :fetcher github :repo "junghan0611/txl.el" :branch "ko")) ;; tmalsburg
        ;; (deepl :location (recipe :fetcher github :repo "emacs-openai/deepl"))
        ;; immersive-translate

        ;; (typst-ts-mode :location (recipe :fetcher sourcehut :repo "meow_king/typst-ts-mode"))

        palimpsest
        olivetti
        logos
        org-translate
        separedit ; Edit comment/string/docstring/code block in separate buffer with your favorite mode.
        ;; sentex ; Regex-based sentence navigation rules
        ;; substitute
        ;; speed-type

        ;; selectric-mode
        wiki-summary
        ;; (wiki-summary :location (recipe :fetcher github :repo "rnkn/wiki-summary.el"))
        define-it
        lexic
        ;; mw-thesaurus
        ;; sdcv
        external-dict
        define-word ; copy from spacemacs-language

        (sdcv :location (recipe :fetcher github :repo "manateelazycat/sdcv"))
        wordreference
        powerthesaurus
        (wiktionary-bro :location (recipe :fetcher github :repo "agzam/wiktionary-bro.el"))

        ;; obsidian
        ;; (use-package reverso
        ;;   :straight (:host github :repo "SqrtMinusOne/reverso.el")
        ;; khoj

        ;; lingva ; tracking-free alternative front-end for Google translate
        ;; libretranslate API, a libre and self-hostable translation engine.
        ;; (libretrans :location
        ;;             (recipe
        ;;             :fetcher url
        ;;             :url "https://codeberg.org/martianh/libretrans.el/raw/branch/main/libretrans.el"))
        )
      )
    ```

<!--list-separator-->

2.  Configuration



    <!--list-separator-->

    1.  Paragraph and Spacing



        <!--list-separator-->

        1.  Paragraphs and fill-mode



            ```elisp
            ;;;; Paragraph and Spacing

            ;;;;; Paragraphs and fill-mode

            ;; 다음은 기본 값이다. 확인 용도
            ;; (setq sentence-end-without-space "。．？")
            ;; (setq sentence-end-without-period nil)
            ;; (setq colon-double-space nil)
            ;; (setq use-hard-newlines nil)

            ```

        <!--list-separator-->

        2.  Multiple-Cursors and Phi-Search



            ```elisp
            ;;;;; multiple-cursors and phi-search

            ;; Multiple cursors is fun and provides quick feedback, allowing for visual
            ;; inspection of the result as you change it.  phi-search is useful for this.  But
            ;; it doesn't work on long files, so let's bind it to special-commands.

            ;; 여러 개의 커서를 사용하면 재미있고 빠른 피드백이 제공되므로 결과를 변경하면서
            ;; 시각적으로 확인할 수 있습니다. phi-search 이 경우에 유용합니다. 하지만 긴
            ;; 파일에서는 작동하지 않으므로 특수 명령에 바인딩해 봅시다.

            (defun jh-writing/init-phi-search ()
              (use-package phi-search :ensure t :defer 4))

            ```

        <!--list-separator-->

        3.  Pangu-Spacing



            ```elisp
            ;;;;; pangu-spacing

            ;; /home/junghan/nosync/emacs-pkgs/emacs-29-git/lisp/emacs-lisp/rx.el
            ;; /home/junghan/spacemacs/layers/+intl/japanese/packages.el
            (defun jh-writing/init-pangu-spacing ()
              (use-package pangu-spacing
                :ensure t
                :init
                (progn ;; replacing `chinese-two-byte' by `japanese'
                  (setq pangu-spacing-include-regexp
                        ;; we didn't add korean because korean-hangul-two-byte is not implemented
                        (rx (or (and (or (group-n 3 (any "。，！？；：「」（）、"))
                                         (group-n 1 (or (in "가-힣")
                                                        ;; (category korean-hangul-two-byte)
                                                        (category chinse-two-byte)
                                                        ;; (category japanese-hiragana-two-byte)
                                                        ;; (category japanese-katakana-two-byte)
                                                        )))
                                     (group-n 2 (in "a-zA-Z0-9")))
                                (and (group-n 1 (in "a-zA-Z0-9"))
                                     (or (group-n 3 (any "。，！？；：「」（）、"))
                                         (group-n 2 (or (in "가-힣")
                                                        ;; (category korean-hangul-two-byte)
                                                        (category chinse-two-byte)
                                                        ;; (category japanese-hiragana-two-byte)
                                                        ;; (category japanese-katakana-two-byte)
                                                        )))))))
                  (spacemacs|hide-lighter pangu-spacing-mode)
                  ;; Always insert `real' space in text-mode including org-mode.
                  (setq pangu-spacing-real-insert-separtor t)
                  ;; (global-pangu-spacing-mode 1) ; no
                  ;; markdown-mode 추가하지마라!
                  (add-hook 'org-mode-hook 'pangu-spacing-mode)
                  )
                )
              )

            ;; (defun japanese/post-init-org ()
            ;;   (defadvice org-html-paragraph (before org-html-paragraph-advice
            ;;                                         (paragraph contents info) activate)
            ;;     "Join consecutive Japanese lines into a single long line without
            ;; unwanted space when exporting org-mode to html."
            ;;     (let* ((origin-contents (ad-get-arg 1))
            ;;            (fix-regexp "[[:multibyte:]]")
            ;;            (fixed-contents
            ;;             (replace-regexp-in-string
            ;;              (concat
            ;;               "\\(" fix-regexp "\\) *\n *\\(" fix-regexp "\\)") "\\1\\2" origin-contents)))
            ;;       (ad-set-arg 1 fixed-contents))))

            ```

        <!--list-separator-->

        4.  Unfill



            ```elisp
            ;;;;; unfill
            (defun jh-writing/init-unfill ()
              (require 'unfill)
              (global-set-key (kbd "C-M-q") 'unfill-paragraph))

            ;; DONE isearch minibuffer 에서 이동할 방법이 없어서 끈다.
            ;; Emacs 기본 C-f/b forward-char backward-char 가 가능한지 검토 바람
            ;; copy from DW
            ;; (defun dw/dont-arrow-me-bro ()
            ;;   (interactive)
            ;;   (message "Arrow keys are bad, you know?"))
            ;; ;; Disable arrow keys in normal and visual modes
            ;; (define-key evil-normal-state-map (kbd "<left>") 'dw/dont-arrow-me-bro)
            ;; (define-key evil-normal-state-map (kbd "<right>") 'dw/dont-arrow-me-bro)
            ;; (define-key evil-normal-state-map (kbd "<down>") 'dw/dont-arrow-me-bro)
            ;; (define-key evil-normal-state-map (kbd "<up>") 'dw/dont-arrow-me-bro)
            ;; (evil-global-set-key 'motion (kbd "<left>") 'dw/dont-arrow-me-bro)
            ;; (evil-global-set-key 'motion (kbd "<right>") 'dw/dont-arrow-me-bro)
            ;; (evil-global-set-key 'motion (kbd "<down>") 'dw/dont-arrow-me-bro)
            ;; (evil-global-set-key 'motion (kbd "<up>") 'dw/dont-arrow-me-bro)

            ;; C-g 기본 함수는 keyboard-quit
            ;; C-g back to normal state
            ;; (define-key evil-insert-state-map (kbd "C-g") 'evil-normal-state)
            ```

        <!--list-separator-->

        5.  Pulsar



            ```elisp
            ;;;;; pulsar

            (defun jh-writing/init-pulsar ()
              (use-package pulsar
                :if (not (or my/remote-server *is-termux*))
                ;; :if window-system
                :init
                ;; (setq pulsar-delay 0.05)
                (setq pulsar-face 'pulsar-magenta)
                (setq pulsar-highlight-face 'pulsar-yellow)
                ;; :config
                ;; (pulsar-global-mode 1)
                ;; (add-hook 'prog-mode-hook 'pulsar-mode)
                )
              )

            ```

        <!--list-separator-->

        6.  Undo-fu



            ```elisp
            ;;;;; undo-fu

            ;; (use-package undohist :after undo-fu
            ;;   :config (undohist-initialize))

            (defun jh-writing/init-undo-fu ()
              ;; Increase undo history limits even more
              (use-package undo-fu
                :demand t
                :config
                ;; C-r 은 isearch-backward 가 기본
                (define-key evil-normal-state-map "u" 'undo-fu-only-undo)
                (define-key evil-normal-state-map "\C-r" 'undo-fu-only-redo)

                ;; (evil-define-key 'normal 'global (kbd "C-r") #'undo-fu-only-redo)
                ;; (evil-define-key 'normal 'global "u" #'undo-fu-only-undo)

                ;; Undo-fu customization options
                ;; Undoing with a selection will use undo within that region.
                (setq undo-fu-allow-undo-in-region t)
                ;; Use the `undo-fu-disable-checkpoint' command instead of Ctrl-G `keyboard-quit' for non-linear behavior.
                (setq undo-fu-ignore-keyboard-quit t)
                ;; By default while in insert all changes are one big blob. Be more granular
                (setq evil-want-fine-undo t)

                (setq evil-undo-system 'undo-fu)
                (evil-set-undo-system 'undo-fu)
                )
              )

            ```

        <!--list-separator-->

        7.  Guess-Language



            ```elisp

            ;;;;; guess-language

            (defun jh-writing/init-guess-language ()
              (use-package guess-language
                :if (not (or my/remote-server *is-termux*))
                :config
                (setq guess-language-langcodes
                      '((en . ("en" "English" "🇬🇧" "English"))
                        (ko . ("ko" "Korean" "🇰🇷" "Korean"))))

                (setq guess-language-languages '(ko en))
                (setq guess-language-min-paragraph-length 35)
                )
              ;; (setq guess-language-trigrams-directory "/home/junghan/sync/emacs/guess-language/trigrams/")
              )

            ;; 여기에 flyspell 언어 바꿔서 해주면 좋겠다.
            ;; (defun my-custom-function (lang beginning end)
            ;;   (do-something))
            ;; (add-hook 'guess-language-after-detection-functions #'my-custom-function)
            ;; (add-hook 'org-mode-hook (lambda () (guess-language-mode 1)))
            ```

        <!--list-separator-->

        8.  visual-fill-column



            ```elisp

            ;;;;; visual-fill-column

            (defun jh-writing/init-visual-fill-column ()
              (use-package visual-fill-column
                :commands visual-fill-column-mode
                :hook ((eww-after-render . visual-fill-column-mode)
                       (eww-after-render . visual-line-mode)
                       ;; (notmuch-show-mode . visual-fill-column-mode)
                       )
                :config
                (setq-default visual-fill-column-center-text t
                              visual-fill-column-width 80)
                )
              )
            ```

        <!--list-separator-->

        9. <span class="org-todo done DONT">DONT</span>  Move-Text



            ```elisp
            ;;;;; move-text

            ;; (defun jh-writing/init-move-text ()
            ;;   (use-package move-text :after evil
            ;;     :bind (:map evil-visual-state-map
            ;;                 ("C-j" . move-text-down)
            ;;                 ("C-k" . move-text-up))
            ;;     ))
            ```

    <!--list-separator-->

    2.  Modal Editing : Evil



        <!--list-separator-->

        1.  Post Evil Tunning



            ```elisp
            ;;;; Editing : Evil

            ;;;;; Post Evil
            (defun jh-writing/post-init-evil ()
              (setq evil-want-C-i-jump t) ; use C-i / C-o  evil-jump-backward/forward
              ;; C-h is backspace in insert state
              (setq evil-want-C-h-delete t)
              (setq evil-want-C-w-delete t) ; default t
              (setq evil-want-C-u-scroll t) ; default nil

              ;;  /home/junghan/sync/man/dotsamples/vanilla/mpereira-dotfiles-evil-clojure/configuration.org
              ;; FIXME: this correctly causes '*' to match on whole symbols (e.g., on a
              ;; Clojure file pressing '*' on 'foo.bar' matches the whole thing, instead of
              ;; just 'foo' or 'bar', BUT, it won't match 'foo.bar' in something like
              ;; '(foo.bar/baz)', which I don't like.
              ;; (setq-default evil-symbol-word-search t)
              ;; (setq evil-jumps-cross-buffers nil)
              (setq evil-want-Y-yank-to-eol t)

              (setq evil-shift-width tab-width)
              ;; (setq evil-search-module 'evil-search)

              ;; (setq evil-complete-all-buffers nil) ; default t
              ;; (setq evil-auto-indent nil) ; default t ; o O insert and indent

              ;; 'Important' Prevent the cursor from moving beyond the end of line.
              ;; Don't move the block cursor when toggling insert mode
              (setq evil-move-cursor-back nil) ; nil is better - default t
              (setq evil-move-beyond-eol nil) ; default nil

              ;; Don't put overwritten text in the kill ring
              (setq evil-kill-on-visual-paste nil) ; better

              ;; Don't create a kill entry on every visual movement.
              ;; More details: https://emacs.stackexchange.com/a/15054:
              (fset 'evil-visual-update-x-selection 'ignore)

              ;; (setq evil-normal-state-cursor '(box "orange")
              ;;       evil-insert-state-cursor '(box "green")
              ;;       evil-visual-state-cursor '(box "#F86155")
              ;;       evil-emacs-state-cursor  '(box "purple"))

              ;; Prevent evil-motion-state from shadowing previous/next sexp
              (require 'evil-maps)
              (define-key evil-motion-state-map "L" nil)
              (define-key evil-motion-state-map "M" nil)
              )
            ```

        <!--list-separator-->

        2.  Evil-Matchit



            ```elisp
            ;;;;; DONT evil-matchit

            ;; 절대 글로벌로 켜지 말 것! 각 스페이스맥스 프로그래밍 언어 레이어에 보면 이미 들어가 있다.
            ;; /mpereira-dotfiles-evil-clojure/configuration.org
            ;; (defun jh-writing/post-init-evil-matchit ()

            ;;   ;; https://github.com/redguardtoo/evil-matchit/pull/141
            ;;   (evilmi-load-plugin-rules '(js-mode
            ;;                               json-mode
            ;;                               js2-mode
            ;;                               js3-mode
            ;;                               javascript-mode
            ;;                               rjsx-mode
            ;;                               js2-jsx-mode
            ;;                               react-mode
            ;;                               typescript-mode
            ;;                               typescript-tsx-mode
            ;;                               tsx-ts-mode)
            ;;                             '(simple javascript html))
            ;;   )
            ```

        <!--list-separator-->

        3.  Evil-Surround



            ```elisp

            ;;;;; evil-surround

            ;; @call-function
            ;; visual mode S- or gS-
            ;; normal mode ys- or yS-
            ;; change surround cs-
            ;; delete surround ds-
            ;; @select area
            ;; call-functionu- - ;현재부터 단어 끝까지
            ;; {call-function}-i- ;현재 단어
            ;; {call-function}-s- ;현재 줄
            ;; @wrap function
            ;; {select-area}-w
            ;; ${target}( 바꾸고싶은거 ), ${change}(바뀔거)
            ;; 감싸기:     => y-s-i-w-${change}( "(", "{", "[")
            ;; 전부 감싸기 => y-s-s-${change}
            ;; 바꾸기: => c-s-${target}( "(", "{", "["), ${change}
            ;; 벗기기: => d-s-${target}( "(", "{", "[")

            ```

        <!--list-separator-->

        4.  Evil-Lion



            ```elisp
            ;;;;; evil-lion

            ;; gl ${operator}
            (defun jh-writing/post-init-evil-lion ()
              (evil-lion-mode +1)
              )

            ```

        <!--list-separator-->

        5.  Evil-Visualstar



            ```elisp
            ;;;;; evil-visualstar

            (defun jh-writing/post-init-evil-visualstar ()
              (setq evil-visualstar/persistent t) ; need
              (global-evil-visualstar-mode t)
              )

            ```

        <!--list-separator-->

        6.  Evil-Goggles



            ```elisp

            ;;;;; evil-goggles

            (defun jh-writing/init-evil-goggles ()
              (use-package evil-goggles
                :ensure t
                :config

                ;; this variable affects "blocking" hints, for example when deleting - the hint is displayed,
                ;; the deletion is delayed (blocked) until the hint disappers, then the hint is removed and the
                ;; deletion executed; it makes sense to have this duration short
                (setq evil-goggles-blocking-duration 0.100) ;; default is nil, i.e. use `evil-goggles-duration'

                ;; this variable affects "async" hints, for example when indenting - the indentation
                ;; is performed with the hint visible, i.e. the hint is displayed, the action (indent) is
                ;; executed (asynchronous), then the hint is removed, highlighting the result of the indentation
                (setq evil-goggles-async-duration 0.900) ;; default is nil, i.e. use `evil-goggles-duration'

                ;; optionally use diff-mode's faces; as a result, deleted text
                ;; will be highlighed with `diff-removed` face which is typically
                ;; some red color (as defined by the color theme)
                ;; other faces such as `diff-added` will be used for other actions
                (evil-goggles-use-diff-faces)

                ;; to disable the hint when pasting:
                (setq evil-goggles-enable-paste nil)
                (setq evil-goggles-enable-yank nil)

                ;; list of all on/off variables, their default value is `t`:
                ;;
                ;; evil-goggles-enable-delete
                ;; evil-goggles-enable-change
                ;; evil-goggles-enable-indent
                ;; evil-goggles-enable-yank
                ;; evil-goggles-enable-join
                ;; evil-goggles-enable-fill-and-move
                ;; evil-goggles-enable-paste
                ;; evil-goggles-enable-shift
                ;; evil-goggles-enable-surround
                ;; evil-goggles-enable-commentary
                ;; evil-goggles-enable-nerd-commenter
                ;; evil-goggles-enable-replace-with-register
                ;; evil-goggles-enable-set-marker
                ;; evil-goggles-enable-undo
                ;; evil-goggles-enable-redo
                ;; evil-goggles-enable-record-macro

                (evil-goggles-mode)
                )
              )

            ```

        <!--list-separator-->

        7.  Evil-String-Inflection



            ```elisp

            ;;;;; evil-string-inflection

            (defun jh-writing/init-evil-string-inflection ()
              (use-package evil-string-inflection
                :config (define-key evil-normal-state-map "gR" 'evil-operator-string-inflection)
                ))

            ```

        <!--list-separator-->

        8.  Evil-Escape



            ```elisp

            ;;;;; evil-escape

            (defun jh-writing/post-init-evil-escape ()
              ;; evil-escape - switch between insert and normal state
              ;; fd 는 ㄹㅇ일 때 적용이 안되니 ,.을 입력 시 escape 하도록 바꿈.
              ;; unordered 로 해보니 minor-mode 를 열기도 해서 아예 논란이 없도록 바꿈.
              (setq-default evil-escape-key-sequence ",.")
              (setq-default evil-escape-unordered-key-sequence nil)
              (setq-default evil-escape-delay 1.0) ;; 0.5, default 0.1
              ;; (setq-default evil-escape-inhibit-functions nil)
              (evil-escape-mode)
              ;; (add-to-list 'evil-escape-excluded-major-modes 'code-review-mode)
              )

            ```

        <!--list-separator-->

        9. <span class="org-todo done DONT">DONT</span>  Evil-Owl



            ```elisp
            ;;;;; evil-owl : mark and register view

            ;; Press q, @, ​"​, C-r, m, ​'​, or ` to view the popup, press C-f or C-b to scroll
            ;; it, and input a register or mark to make the popup disappear.

            ;; (defun jh-writing/init-evil-owl ()
            ;;   (use-package evil-owl
            ;;     :if (not (or my/remote-server *is-termux*))
            ;;     :config
            ;;     (setq evil-owl-idle-delay 0.5)
            ;;     (setq evil-owl-max-string-length 500)

            ;;     ;; (when (display-graphic-p) ; gui
            ;;     ;;   (setq evil-owl-display-method 'posframe
            ;;     ;;         evil-owl-extra-posframe-args '(:width 50 :height 20)
            ;;     ;;         evil-owl-max-string-length 50))

            ;;     (evil-owl-mode)
            ;;     )
            ;;   )
            ```

        <!--list-separator-->

        10. <span class="org-todo done DONT">DONT</span>  Evil-Traces



            ```elisp
            ;;;;; TODO evil-traces

            ;; move: m +{n}, delete: +{n},+{n}d, join: .,+{n}j glboal: g/{target}/{change}
            ;; (defun jh-writing/init-evil-traces ()
            ;;   (use-package evil-traces :after evil
            ;;     :if (not (or my/remote-server *is-termux*))
            ;;     :config (evil-traces-use-diff-faces)
            ;;     (evil-traces-mode)
            ;;     )
            ;;   )
            ```

    <!--list-separator-->

    3.  Search and Replace



        <!--list-separator-->

        1.  Ripgrep : rg



            ```elisp
            ;;;; Search and Replace

            ;;;;; ripgrep + wgrep

            (defun jh-writing/init-rg ()
              (use-package rg
                :after consult
                :defer 10
                :config
                ;; (rg-enable-default-bindings) ;; use =C-c s=
                (rg-enable-menu) ; magit style
                (setq rg-command-line-flags '("--hidden" "--follow"))

                ;; (setq rg-group-result t
                ;;       rg-hide-command t
                ;;       rg-show-columns nil
                ;;       rg-show-header t
                ;;       rg-custom-type-aliases nil
                ;;       rg-default-alias-fallback "all")

                ;; 버퍼가 열리면 포커스를 그쪽으로 이동시킨다.
                ;; 이거 없으면 생각보다 귀찮아진다.
                (add-hook 'rg-mode-hook (lambda () (switch-to-buffer-other-window "*rg*")))

                ;; (rg-define-toggle "--multiline --multiline-dotall" "u")
                ;; (rg-define-toggle "--word-regexp" "w")
                ;; (rg-define-toggle "--files-with-matches" "L")
                ;; (rg-define-toggle "--files-without-match @")

                (rg-define-search rg-files-without-match
                  :format literal
                  :flags ("--files-without-match")
                  :menu ("Custom" "@" "Files without matches"))

                ;; (rg-define-search my/rg-org-directory
                ;;   :query ask
                ;;   :format regexp
                ;;   :files "org"
                ;;   :dir org-roam-directory
                ;;   :confirm prefix)

                ;; (rg-define-search my/rg-vc-or-dir
                ;;   "RipGrep in project root or present directory."
                ;;   :query ask
                ;;   :format regexp
                ;;   :files "everything"
                ;;   :dir (or (project-root (project-current))
                ;;            (vc-root-dir)              ; search root project dir
                ;;            default-directory)         ; or from the current dir
                ;;   :confirm prefix
                ;;   :flags ("--hidden -g !.git"))

                ;; (rg-define-search my/rg-ref-in-dir
                ;;   "RipGrep for thing at point in present directory."
                ;;   :query point
                ;;   :format regexp
                ;;   :files "everything"
                ;;   :dir default-directory
                ;;   :confirm prefix
                ;;   :flags ("--hidden -g !.git"))
                )
              )

            ```

        <!--list-separator-->

        2.  deadgrep



            ```elisp
            ;;;;; deadgrep

            (defun jh-writing/init-deadgrep ()
              (use-package deadgrep :after consult
                :custom
                (deadgrep-project-root-function 'projectile-project-root)
                :defer 10))

            ```

        <!--list-separator-->

        3.  find-file-in-project



            ```elisp
            ;;;;; find-file-in-project

            (defun jh-writing/init-find-file-in-project ()
              ;; In emacs, run M-x find-file-in-project-by-selected to find matching files.
              ;; Alternatively, run M-x find-file-in-project to list all available files in
              ;; the project.
              (use-package find-file-in-project
                :init
                (setq ffip-use-rust-fd t)
                :config
                ;; ffip adds `ffap-guess-file-name-at-point' automatically and it is crazy
                ;; slow on TRAMP buffers.
                (remove-hook 'file-name-at-point-functions 'ffap-guess-file-name-at-point)
                )
              )
            ```

        <!--list-separator-->

        4.  affe : async fuzzy finder



            ```elisp

            ;;;;; async fuzzy finder

            (defun jh-writing/init-affe ()
              (use-package affe
                :after consult
                :defer 5
                :config
                ;; Use orderless to compile regexps
                (defun +affe-orderless-regexp-compiler (input _type _ignorecase)
                  (setq input (orderless-pattern-compiler input))
                  (cons input (lambda (str) (orderless--highlight input str))))
                (setq affe-regexp-compiler #'+affe-orderless-regexp-compiler)

                ;; Manual preview keys
                (consult-customize
                 affe-grep affe-find
                 :preview-key '("M-." "C-SPC"
                                :debounce 0.3 "<up>" "<down>" "C-n" "C-p"
                                ))
                )
              )
            ```

        <!--list-separator-->

        5.  visual-regexp



            ```elisp

            ;;;;; visual-regexp

            (defun jh-writing/init-visual-regexp ()
              ;; EMACS-style regex support
              (require 'visual-regexp)
              )

            ;; (setq vr/match-separator-use-custom-face t)
            ;; if you use multiple-cursors, this is for you:
            ;; (define-key global-map (kbd "C-c v m") 'vr/mc-mark)
            ;; C-M-r isearch-backward-regexp : default binding
            ;; C-M-s isearch-forward-regexp
            ```

    <!--list-separator-->

    4.  Structural Editing



        <!--list-separator-->

        1.  Puni



            ```elisp

            ;;;; Structural Editing : Puni

            ;; /man/dotsamples/vanilla/prot-old-dotfiles/prot-emacs.el
            ;; '(("' Single quote"        . (39 39))     ; ' '
            ;;   ("\" Double quotes"      . (34 34))     ; " "
            ;;   ("` Elisp quote"         . (96 39))     ; ` '
            ;;   ("‘ Single apostrophe"   . (8216 8217)) ; ‘ ’
            ;;   ("“ Double apostrophes"  . (8220 8221)) ; “ ”
            ;;   ("( Parentheses"         . (40 41))     ; ( )
            ;;   ("{ Curly brackets"      . (123 125))   ; { }
            ;;   ("[ Square brackets"     . (91 93))     ; [ ]
            ;;   ("< Angled brackets"     . (60 62))     ; < >
            ;;   ("« Εισαγωγικά Gr quote" . (171 187))   ; « »
            ;;   ("= Equals signs"        . (61 61))     ; = =
            ;;   ("~ Tilde"               . (126 126))   ; ~ ~
            ;;   ("* Asterisks"           . (42 42))     ; * *
            ;;   ("/ Forward Slash"       . (47 47))     ; / /
            ;;   ("_ underscores"         . (95 95))))   ; _ _

            ;;;;; Puni

            (defun jh-writing/init-puni ()
              (use-package puni
                :diminish ""
                :hook ((puni-mode  . electric-pair-mode)
                       (prog-mode  . puni-mode))
                :init
                ;; The default `puni-mode-map' respects "Emacs conventions".  We don't, so
                ;; it's better to simply clear and rewrite it.
                (setcdr puni-mode-map nil)

                (require 'lib-puni)

                (bind-keys
                 :map puni-mode-map

                 ;; ("M-<backspace>" . puni-splice)
                 ("M-<delete>" . puni-splice) ; sp-unwrap-sexp

                 ("C-<right>"  .  puni-slurp-forward)
                 ("C-<left>" . puni-barf-forward)

                 ("C-M-<left>" .  puni-slurp-backward)
                 ("C-M-<right>" . puni-barf-backward)

                 ("C-M-<delete>" . puni-splice-killing-forward)
                 ("C-M-<backspace>" . puni-splice-killing-backward)

                 ("C-M-a" . beginning-of-defun) ; default
                 ("C-M-e" . end-of-defun)
                 ("M-]" . forward-sexp) ; default
                 ("M-[" . backward-sexp)

                 ("C-M-f" . puni-forward-sexp)
                 ("C-M-b" . puni-backward-sexp)

                 ("C-M-p" . puni-beginning-of-sexp)
                 ("C-M-n" . puni-end-of-sexp)

                 ;; C-M-d down-sexp
                 ("C-M-t" . transpose-sexp)
                 ("C-M-?" . puni-convolute)

                 ("C-M-k" . kill-sexp)
                 ("C-M-K"   . backward-kill-sexp)
                 ;; ("C-" . puni-backward-kill-word)

                 ("M-)" . puni-syntactic-forward-punct)
                 ("M-(" . puni-syntactic-backward-punct)

                 ("C-c DEL" . puni-force-delete)
                 ;; ("C-M-d" . puni-forward-delete-char)
                 ;; ("C-M-k" . puni-kill-line)
                 ;; ("C-M-K" . puni-backward-kill-line)
                 ;;  ("C-M-w" . puni-kill-region)

                 ;; ([remap puni-backward-kill-word] . backward-kill-word)
                 ("C-M-z" . puni-squeeze) ; unwrap

                 ("C-c {" . puni-wrap-curly)
                 ("C-c (" . puni-wrap-round)
                 ("C-c [" . puni-wrap-square)
                 )
                )
              )

            ;;   :config
            ;;   (defun puni-kill-thing-at-point (&optional arg)
            ;;     "Kill the next puni based thing at point"
            ;;     (interactive)
            ;;     (unless buffer-read-only
            ;;       (puni-expand-region)
            ;;       (kill-region (region-beginning) (region-end))))

            ;;   (defun puni-clone-thing-at-point (&optional arg)
            ;;     "Clone the next puni based thing at point"
            ;;     (interactive)
            ;;     (save-excursion
            ;;       (puni-expand-region)
            ;;       (kill-ring-save (region-beginning) (region-end)))
            ;;     (yank)
            ;;     (default-indent-new-line))

            ;; ;;;; Better Killing And Yanking
            ;;   (setq rectangle-mark-mode nil)
            ;;   (setq *last-kill-was-rectangle* rectangle-mark-mode)

            ;;   (defun remember-last-kill-type (&rest d)
            ;;     (setq *last-kill-was-rectangle* rectangle-mark-mode))

            ;;   ;; (advice-add 'kill-region :before #'remember-last-kill-type)
            ;;   ;; (advice-add 'kill-ring-save :before #'remember-last-kill-type)
            ;;   ;; (advice-add 'kill-rectangle :before #'remember-last-kill-type)

            ;;   (defun my/kill-region (BEG END &optional REGION)
            ;;     (interactive (list (mark) (point) 'region))
            ;;     (cond
            ;;      (rectangle-mark-mode (kill-rectangle
            ;;                            (region-beginning) (region-end)))
            ;;      (mark-active (kill-region
            ;;                    (region-beginning) (region-end)))
            ;;      (t (backward-kill-sexp 1))))

            ;;   (defun my/yank (&optional arg) (interactive)
            ;;          (if *last-kill-was-rectangle*
            ;;              (yank-rectangle)
            ;;            (yank arg)))
            ;; Avoid terminal binding conflict
            ;; (unless my/is-termux
            ;;   (bind-key (kbd "M-[") #'puni-splice 'puni-mode-map)
            ;;   (bind-key (kbd "M-]") #'puni-split 'puni-mode-map))
            ```

        <!--list-separator-->

        2. <span class="org-todo done DONT">DONT</span>  Smartparens



            ```elisp

            ;;;;; DONT Smartparens

            ;; (defun jh-writing/post-init-smartparens ()
            ;;   (use-package smartparens
            ;;     :ensure
            ;;     :demand
            ;;     ;; :init
            ;;     ;; prog-mode + org/markdown 에서 활용
            ;;     ;; (add-hook 'org-mode-hook #'smartparens-mode)
            ;;     ;; (add-hook 'markdown-mode-hook #'smartparens-mode)
            ;;     :bind (
            ;;            :map
            ;;            smartparens-mode-map
            ;;            ;; fuco default style
            ;;            ("C-M-k" . sp-kill-sexp)
            ;;            ("C-M-w" . sp-copy-sexp)
            ;;            ("M-<delete>" . sp-unwrap-sexp)
            ;;            ("M-<backspace>" . sp-backward-unwrap-sexp)
            ;;            ("C-<right>" . sp-forward-slurp-sexp)
            ;;            ("C-<left>" . sp-forward-barf-sexp)
            ;;            ("C-M-<left>" . sp-backward-slurp-sexp)
            ;;            ("C-M-<right>" . sp-backward-barf-sexp)
            ;;            ("M-D" . sp-splice-sexp)
            ;;            ("C-M-<delete>" . sp-splice-sexp-killing-forward)
            ;;            ("C-M-<backspace>" . sp-splice-sexp-killing-backward)
            ;;            ("C-S-<backspace>" . sp-splice-sexp-killing-around)
            ;;            ("M-F" . sp-forward-symbol)
            ;;            ("M-B" . sp-backward-symbol)

            ;;            ;; my custom
            ;;            ("C-M-f" . forward-sexp)
            ;;            ("C-M-b" . backward-sexp)
            ;;            ("C-M-a" . sp-beginning-of-sexp)
            ;;            ("C-M-e" . sp-end-of-sexp)
            ;;            ("C-M-n" . sp-next-sexp)
            ;;            ("C-M-p" . sp-previous-sexp)

            ;;            ("C-M-t" . sp-transpose-sexp)
            ;;            ("C-c {" . sp-wrap-curly)
            ;;            ("C-c (" . sp-wrap-round)
            ;;            ("C-c [" . sp-wrap-square)
            ;;            )
            ;;     :config

            ;;     ;; 2023-09-14 global 로 사용하다보니 거슬린다. 잠시만. 글로벌을 빼면 어떤가?
            ;;     ;; ("\\\\(" . "\\\\)") ;; emacs regexp parens
            ;;     ;; ("\\{"   . "\\}")   ;; latex literal braces in math mode
            ;;     ;; ("\\("   . "\\)")   ;; capture parens in regexp in various languages
            ;;     ;; ("\\\""  . "\\\"")  ;; escaped quotes in strings
            ;;     ;; ("/*"    . "*/")    ;; C-like multi-line comment
            ;;     ;; ("\""    . "\"")    ;; string double quotes
            ;;     ;; ("'"     . "'")     ;; string single quotes/character quotes
            ;;     ;; ("("     . ")")     ;; parens (yay lisp)
            ;;     ;; ("["     . "]")     ;; brackets
            ;;     ;; ("{"     . "}")     ;; braces (a.k.a. curly brackets)
            ;;     ;; ("`"     . "`")     ;; latex strings. tap twice for latex double quotes

            ;;     ;; Unbind `M-s' (set by paredit keybindings above) because it's bound
            ;;     ;; to some handy occur related functions
            ;;     (define-key sp-keymap (kbd "M-s") nil)

            ;;     ;; org 모드에서 거슬린다. 제거. 굳.
            ;;     ;; (sp-local-pair 'org-mode "=" "=") ; 아마 들어가 있을듯
            ;;     (sp-local-pair 'org-mode "[" "]" :actions '(rem)) ;; 삭제
            ;;     (sp-local-pair 'org-mode "'" "'" :actions '(rem))
            ;;     (sp-local-pair 'org-mode "`" "`" :actions '(rem))
            ;;     (sp-local-pair 'org-mode "\"" "\"" :actions '(rem))
            ;;     (sp-local-pair 'org-mode "/" "/" :actions '(rem))
            ;;     (sp-local-pair 'org-mode "=" "=" :actions '(rem))
            ;;     (sp-local-pair 'org-mode "~" "~" :actions '(rem))

            ;;     ;; markdown 에서도 삭제
            ;;     (sp-local-pair 'markdown-mode "'" "'" :actions '(rem))
            ;;     (sp-local-pair 'markdown-mode "`" "`" :actions '(rem))
            ;;     (sp-local-pair 'markdown-mode "\"" "\"" :actions '(rem))
            ;;     (sp-local-pair 'markdown-mode "/" "/" :actions '(rem))

            ;;     ;; pair management
            ;;     (sp-with-modes '(minibuffer-mode)
            ;;       (sp-local-pair "'" nil :actions nil)
            ;;       (sp-local-pair "(" nil :wrap "C-("))

            ;;     (sp-with-modes 'markdown-mode
            ;;       (sp-local-pair "**" "***"))

            ;;     (sp-with-modes 'web-mode
            ;;       (sp-local-pair "{{#if" "{{/if")
            ;;       (sp-local-pair "{{#unless" "{{/unless"))

            ;;     ;; lisp modes
            ;;     ;; (sp-with-modes sp--lisp-modes
            ;;     ;;   (sp-local-pair "(" nil
            ;;     ;;                  :wrap "C-("
            ;;     ;;                  :pre-handlers '(my-add-space-before-sexp-insertion)
            ;;     ;;                  :post-handlers '(my-add-space-after-sexp-insertion)))

            ;;     ;; (defun my-add-space-after-sexp-insertion (id action _context)
            ;;     ;;   (when (eq action 'insert)
            ;;     ;;     (save-excursion
            ;;     ;;       (forward-char (sp-get-pair id :cl-l))
            ;;     ;;       (when (or (eq (char-syntax (following-char)) ?w)
            ;;     ;;                 (looking-at (sp--get-opening-regexp)))
            ;;     ;;         (insert " ")))))

            ;;     ;; (defun my-add-space-before-sexp-insertion (id action _context)
            ;;     ;;   (when (eq action 'insert)
            ;;     ;;     (save-excursion
            ;;     ;;       (backward-char (length id))
            ;;     ;;       (when (or (eq (char-syntax (preceding-char)) ?w)
            ;;     ;;                 (and (looking-back (sp--get-closing-regexp))
            ;;     ;;                      (not (eq (char-syntax (preceding-char)) ?'))))
            ;;     ;;         (insert " ")))))

            ;;     ;; ;; SP config for other modes. (from vedang)
            ;;     ;; (eval-after-load 'cider-repl
            ;;     ;;   '(progn
            ;;     ;;      (define-key cider-repl-mode-map (kbd ")") 'sp-up-sexp)
            ;;     ;;      (define-key cider-repl-mode-map (kbd "]") 'sp-up-sexp)
            ;;     ;;      (define-key cider-repl-mode-map (kbd "}") 'sp-up-sexp)))

            ;;     ;; (eval-after-load 'clojure-mode
            ;;     ;;   '(progn
            ;;     ;;      (define-key clojure-mode-map (kbd ")") 'sp-up-sexp)
            ;;     ;;      (define-key clojure-mode-map (kbd "]") 'sp-up-sexp)
            ;;     ;;      (define-key clojure-mode-map (kbd "}") 'sp-up-sexp)))

            ;;     ;; indent after inserting any kinds of parens
            ;;     ;; (defun my/smartparens-pair-newline-and-indent (id action context)
            ;;     ;;   (save-excursion
            ;;     ;;     (newline)
            ;;     ;;     (indent-according-to-mode))
            ;;     ;;   (indent-according-to-mode))
            ;;     ;; (sp-pair "(" nil :post-handlers
            ;;     ;;          '(:add (my/smartparens-pair-newline-and-indent "RET")))
            ;;     ;; (sp-pair "{" nil :post-handlers
            ;;     ;;          '(:add (my/smartparens-pair-newline-and-indent "RET")))
            ;;     ;; (sp-pair "[" nil :post-handlers
            ;;     ;;          '(:add (my/smartparens-pair-newline-and-indent "RET")))

            ;;     )
            ;;   )
            ```

    <!--list-separator-->

    5.  File-Format and Integration



        ```elisp

        ;;;; Markdown

        ;; format-all 등록
        ;; (defun jh-writing/pre-init-markdown-mode ()
        ;;   (spacemacs|use-package-add-hook markdown-mode
        ;;     :pre-init
        ;;     (defun markdown-formatting-hook ()
        ;;       (setq-local format-all-formatters '(("Markdown" prettierd)))) ; prettier deno
        ;;     (add-hook 'markdown-mode-hook #'markdown-formatting-hook)
        ;;     )
        ;;   )

        ;; check this - outline-level 'markdown-outline-level
        (defun jh-writing/post-init-markdown-mode ()
          ;; :bind (:map
          ;;        markdown-mode-map
          ;;        ("C-<tab>" . outline-hide-entry)
          ;;        ("M-j" . markdown-next-visible-heading)
          ;;        ("M-k" . markdown-previous-visible-heading)
          ;;        ;; ("M-<up>" . markdown-move-up) ; conflict smartparens-mode
          ;;        ;; ("M-<down>" . markdown-move-down)
          ;;        ("C-n" . 'markdown-next-visible-heading)
          ;;        ("C-p" .  'markdown-previous-visible-heading)
          ;;        ("C-c C-o" . 'markdown-follow-thing-at-point)
          ;;        ("C-c C-x C-v" . markdown-toggle-inline-images))

          ;; Make markdown-mode behave a bit more like org w.r.t. code blocks i.e.
          ;; use proper syntax highlighting
          (setq markdown-hide-urls nil) ; must
          (setq markdown-fontify-code-blocks-natively t)
          (setq markdown-display-remote-images t)
          (setq markdown-list-item-bullets '("◦" "-" "•" "–"))

          (setq markdown-command
                (concat
                 "pandoc"
                 ;; " --from=markdown --to=html"
                 ;; " --standalone --mathjax --highlight-style=pygments"
                 ;; " --css=~/.pandoc/pandoc.css"
                 ;; " --quiet"
                 ;; " --number-sections"
                 ;; " --lua-filter=~/dotfiles/pandoc/cutsection.lua"
                 ;; " --lua-filter=~/dotfiles/pandoc/cuthead.lua"
                 ;; " --lua-filter=~/dotfiles/pandoc/date.lua"
                 ;; " --metadata-file=~/dotfiles/pandoc/metadata.yml"
                 ;; " --metadata=reference-section-title:References"
                 ;; " --citeproc"
                 ;; " --bibliography=~/Dropbox/Work/bibfile.bib"
                 ))

          (advice-add
           'markdown-fontify-list-items :override
           (lambda (last)
             (when (markdown-match-list-items last)
               (when (not (markdown-code-block-at-point-p (match-beginning 2)))
                 (let* ((indent (length (match-string-no-properties 1)))
                        (level (/ indent markdown-list-indent-width))
                        ;; level = 0, 1, 2, ...
                        (bullet (nth (mod level (length markdown-list-item-bullets))
                                     markdown-list-item-bullets)))
                   (add-text-properties
                    (match-beginning 2) (match-end 2) '(face markdown-list-face))
                   (cond
                    ;; Unordered lists
                    ((string-match-p "[\\*\\+-]" (match-string 2))
                     (add-text-properties
                      (match-beginning 2) (match-end 2) `(display ,bullet)))
                    ;; Definition lists
                    ((string-equal ":" (match-string 2))
                     (let ((display-string
                            (char-to-string (markdown--first-displayable
                                             markdown-definition-display-char))))
                       (add-text-properties (match-beginning 2) (match-end 2)
                                            `(display ,display-string)))))))
               t)))

          ;; hook
          (add-hook 'markdown-mode-hook #'visual-line-mode)
          ;; (add-hook 'markdown-mode-hook #'auto-fill-mode) ;; 2023-12-19 turn-off

          (add-hook
           'markdown-mode-hook
           (lambda ()
             "Beautify Markdown em-dash and checkbox Symbol"
             (push '("---" . "—") prettify-symbols-alist)
             (push '("->" . "⟶" ) prettify-symbols-alist)
             (push '("=>" . "⟹") prettify-symbols-alist)
             (prettify-symbols-mode)))

          ;; Plain text (text-mode)
          (add-to-list 'auto-mode-alist '("\\(README\\|CHANGELOG\\|COPYING\\|LICENSE\\)\\'" . text-mode))

          (define-key markdown-mode-map (kbd "C-<up>") 'markdown-backward-same-level)
          (define-key markdown-mode-map (kbd "C-<down>") 'markdown-forward-same-level)
          (define-key markdown-mode-map (kbd "<f3>") 'markdown-toggle-markup-hiding)
          (define-key markdown-mode-map (kbd "<f4>") 'markdown-toggle-inline-images)

          ;; (define-key markdown-mode-map (kbd "<f9> l") 'markdown-toggle-markup-hiding)
          ;; (define-key markdown-mode-map (kbd "<f9> g") 'markdown-toggle-inline-images)

          (evil-define-key '(insert) markdown-mode-map (kbd "C-u") 'undo-fu-only-undo)
          (evil-define-key '(insert) markdown-mode-map (kbd "C-r") 'undo-fu-only-redo)

          (evil-define-key '(normal insert) markdown-mode-map (kbd "C-n") 'markdown-outline-next)
          (evil-define-key '(normal insert) markdown-mode-map (kbd "C-p") 'markdown-outline-previous)
          )
        ```

        <!--list-separator-->

        1. <span class="org-todo done DONT">DONT</span>  typst



            ```elisp

            ;;;;; DONT typst-ts-mode

            ;; (defun jh-writing/init-typst-ts-mode ()
            ;;   (use-package typst-ts-mode
            ;;     :ensure t
            ;;     :custom
            ;;     (typst-ts-mode-watch-options "--open")
            ;;     :config
            ;;     (with-eval-after-load 'consult
            ;;       (setq
            ;;         consult-imenu-config
            ;;         (append consult-imenu-config
            ;;           '((typst-ts-mode :topLevel "Headings" :types
            ;;               ((?h "Headings" typst-ts-markup-header-face)
            ;;                 (?f "Functions" font-lock-function-name-face))))))
            ;;       )
            ;;     )
            ;;   )
            ```

        <!--list-separator-->

        2. <span class="org-todo done DONT">DONT</span>  Obsidian.el



            ```elisp

            ;;;;; DONT obsidian

            ;; (defun jh-writing/init-obsidian ()
            ;;   (use-package obsidian
            ;;     :defer 12
            ;;     :if (not (or my/remote-server *is-termux*))
            ;;     :custom
            ;;     (obsidian-inbox-directory "_notes")
            ;;     :config
            ;;     ;; (require 'hydra)
            ;;     ;; (bind-key (kbd "M-g O") 'obsidian-hydra/body 'obsidian-mode-map)
            ;;     (setq obsidian-include-hidden-files nil)

            ;;     (obsidian-specify-path "~/git/notes")
            ;;     ;; (global-obsidian-mode t)
            ;;     )
            ;;   )
            ```

    <!--list-separator-->

    6.  Dictionaries



        ```elisp

        ;;;; Dictionaries

        ;;;;; dictionary
        (defun jh-writing/init-dictionary ()
          (use-package dictionary
            :config
            (setq dictionary-server "localhost")
            ;; (setq dictionary-server "dict.org")
            (setq dictionary-default-popup-strategy "lev" ; read doc string
                  dictionary-create-buttons nil
                  dictionary-use-single-buffer t)))

        ;;;;; define-it
        (defun jh-writing/init-define-it ()
          (use-package define-it
            ;; :if (not (or my/remote-server *is-termux*))
            :defer 12
            :config
            (setq
             define-it-show-google-translate nil
             define-it-show-header nil)

            ;; it doesn't pop to the buffer automatically, when definition is fetched
            (defun define-it--find-buffer (x)
              (let ((buf (format define-it--buffer-name-format define-it--current-word)))
                (pop-to-buffer buf)))

            (advice-add 'define-it--in-buffer :after #'define-it--find-buffer)
            (add-to-list
             'display-buffer-alist
             '("\\*define-it:"
               (display-buffer-reuse-window
                display-buffer-in-direction)
               (direction . right)
               (window . root)
               (window-width . 0.25)))
            ))

        ;;;;; lexic
        (defun jh-writing/init-lexic ()
          (use-package lexic
            :if (not (or my/remote-server *is-termux*))
            :defer 10))

        ;;;;; mw-thesaurus
        ;; agzam-dot-spacemacs/layers/ag-lang-tools/packages.el:3
        ;; sdcv-mode is for browsing 'stardict' format dictionaries in Emacs
        ;; to get Webster’s Revised Unabridged Dictionary
        ;; 1) download it from https://s3.amazonaws.com/jsomers/dictionary.zip
        ;; 2) unzip it twice and put into ~/.stardict/dic
        ;; 3) Install sdcv, a command-line utility for accessing StarDict dictionaries

        ;; (defun jh-writing/init-mw-thesaurus ()
        ;;   (use-package mw-thesaurus
        ;;     :defer 8
        ;;     :if (not (or my/remote-server *is-termux*))
        ;;     :config
        ;;     (define-key mw-thesaurus-mode-map [remap evil-record-macro] #'mw-thesaurus--quit)
        ;;     (add-hook 'mw-thesaurus-mode-hook 'variable-pitch-mode)

        ;;     ;; mw-thesaurus--base-url
        ;;     ;; http://www.dictionaryapi.com/api/v1/references/thesaurus/xml/

        ;;     (add-to-list
        ;;      'display-buffer-alist
        ;;      `(,mw-thesaurus-buffer-name
        ;;        (display-buffer-reuse-window
        ;;         display-buffer-in-direction)
        ;;        (direction . right)
        ;;        (window . root)
        ;;        (window-width . 0.3)))
        ;;     )
        ;;   )

        ;;;;; external-dict

        (defun jh-writing/init-external-dict ()
          (use-package external-dict
            :if (not (or my/remote-server *is-termux*))
            :defer 14))

        ;;;;; define-word

        (defun jh-writing/init-define-word ()
          (use-package define-word
            :if (not (or my/remote-server *is-termux*))
            :defer 8))

        ;;;;; sdcv

        (defun jh-writing/init-sdcv ()
          (use-package sdcv
            :defer 6
            :if (not (or my/remote-server *is-termux*))
            :config
            (require 'posframe)
            (face-spec-set 'sdcv-tooltip-face
                           '((((background light))
                              :foreground "#000000" :background "#ffffff" :weight semi-light :height 0.9)
                             (t
                              :foreground "#ffffff" :background "#000000" :weight semi-light :height 0.9))
                           'face-override-spec)

            (add-hook 'sdcv-mode-hook #'visual-line-mode)
            (setq sdcv-tooltip-timeout 10)
            (setq sdcv-env-lang "ko_KR.UTF-8")
            (setq sdcv-tooltip-border-width 2)
            (setq sdcv-say-word-p t)               ; say word after translation
            (setq sdcv-dictionary-data-dir (file-truename "~/.stardict/dic/"))
            (setq sdcv-fail-notify-string "*Not Found*")

            (setq sdcv-dictionary-simple-list    ;; setup dictionary list for simple search
                  '(
                    "Korean Dic"
                    "quick_english-korean" ; 영한 사전
                    "Kor-Eng Dictionary" ; 한영 사전 (quick)
                    "Merrian Webster 10th dictionary"
                    "Hanja(Korean Hanzi) Dic"
                    ))

            (setq sdcv-dictionary-complete-list     ;; setup dictionary list for complete search
                  '(
                    "Korean Dic"
                    "quick_english-korean"
                    "Kor-Eng Dictionary"
                    "Hanja(Korean Hanzi) Dic"
                    "Merrian Webster 10th dictionary"
                    "Webster's Revised Unabridged Dictionary (1913)"
                    "Moby Thesaurus II"
                    ))
            )
          )

        ;;;;; wordreference
        (defun jh-writing/init-wordreference ()
          (use-package wordreference
            :defer 8
            :if (not (or my/remote-server *is-termux*))
            :hook (wordreference-mode . visual-line-mode)
            :commands (wordreference-search
                       my/wr-enko
                       my/wr-koen)
            :config
            (defun my/wr-koen ()
              (interactive)
              (let ((wordreference-source-lang "ko")
                    (wordreference-target-lang "en"))
                (wordreference-search)))
            (defun my/wr-enko ()
              (interactive)
              (let ((wordreference-source-lang "en")
                    (wordreference-target-lang "ko"))
                (wordreference-search)))
            )
          )

        ;;;;; powerthesaurus
        (defun jh-writing/init-powerthesaurus ()
          (use-package powerthesaurus
            :after hydra
            :if (not (or my/remote-server *is-termux*))
            :defer 10))

        ;;;;; wiktionary
        (defun jh-writing/init-wiktionary-bro ()
          (use-package wiktionary-bro
            :defer 10
            :if (not (or my/remote-server *is-termux*))
            :commands (wiktionary-bro-dwim)
            )
          )
        ```

    <!--list-separator-->

    7.  Translator/Translation



        ```elisp

        ;;;; Translate

        ;;;;; wiki-summary
        (defun jh-writing/init-wiki-summary ()
          (use-package wiki-summary :defer 12))

        ;;;;; org-translate
        (defun jh-writing/init-org-translate ()
          (require 'org-translate)
          (setq ogt-default-segmentation-strategy 'paragraph)
          )
        ;;;;; sentex
        ;; (defun jh-writing/init-sentex ()
        ;;   (use-package sentex
        ;;     :if (not (or my/remote-server *is-termux*))
        ;;     :defer 15)
        ;;   )
        ```

        <!--list-separator-->

        1. <span class="org-todo done DONT">DONT</span>  Txl.el



            ```elisp

            ;;;;; txl.el deepl

            ;; (add-to-list 'load-path "~/sync/emacs/forked-pkgs/txl.el/")
            ;; (defun jh-writing/init-txl ()
            ;;   (require 'txl)
            ;;   (setq txl-languages '(EN-US . KO))
            ;;   ;; (setq txl-deepl-api-url "https://api-free.deepl.com/v2/translate") ;; free
            ;;   (setq txl-deepl-api-key my_deepl_apikey)
            ;;   )
            ```

        <!--list-separator-->

        2. <span class="org-todo done DONT">DONT</span>  Immersive-Translate



            ```elisp

            ;;;;; immersive-translate

            ;; Immersive-translate provides bilingual simultaneous display and translation
            ;; of any text in Emacs.
            ;; (defun jh-writing/init-immersive-translate ()
            ;;   (use-package immersive-translate
            ;;     :init
            ;;     (setq immersive-translate-backend 'deepl)
            ;;     (setq immersive-translate-deepl-source-language "EN-US")
            ;;     (setq immersive-translate-deepl-target-language "KO")
            ;;     (setq immersive-translate-auto-idle 2.0) ; default 0.5
            ;;     :config
            ;;     (add-hook 'elfeed-show-mode-hook #'immersive-translate-setup)
            ;;     (add-hook 'nov-mode-hook #'immersive-translate-setup)
            ;;     (add-hook 'Info-mode-hook #'immersive-translate-setup)
            ;;     (add-hook 'help-mode-hook #'immersive-translate-setup)
            ;;     (add-hook 'helpful-mode-hook #'immersive-translate-setup)
            ;;     )
            ;;   )
            ```

        <!--list-separator-->

        3. <span class="org-todo done DONT">DONT</span>  google-translate



            ```elisp

            ;;;; DONT google-translate

            ;; (defun jh-writing/init-google-translate ()
            ;;   (use-package google-translate
            ;;     :commands (spacemacs/set-google-translate-languages)
            ;;     :init
            ;;     ;; fix search fail ',ttk'
            ;;     ;; (see https://github.com/atykhonov/google-translate/issues/52#issuecomment-727920888)
            ;;     (with-eval-after-load 'google-translate-tk
            ;;       (defun google-translate--search-tkk () "Search TKK." (list 430675 2721866130)))
            ;;     (progn
            ;;       (autoload 'google-translate-translate "google-translate-core-ui" "google-translate-translate" nil nil)
            ;;       (autoload 'popup-tip "popup" "popup-tip" nil nil)

            ;;       (defun spacemacs/set-google-translate-languages (&optional override-p)
            ;;         "Set source language for google translate.
            ;; For instance pass En as source for English."
            ;;         (interactive "P")
            ;;         (autoload 'google-translate-read-args "google-translate-default-ui")
            ;;         (let* ((langs (google-translate-read-args override-p nil))
            ;;                (source-language (car langs))
            ;;                (target-language (cadr langs)))
            ;;           (setq google-translate-default-source-language source-language)
            ;;           (setq google-translate-default-target-language target-language)
            ;;           (message
            ;;            (format "Set google translate source language to %s and target to %s"
            ;;                    source-language target-language))))

            ;;       (defun spacemacs/set-google-translate-target-language ()
            ;;         "Set the target language for google translate."
            ;;         (interactive)
            ;;         (spacemacs/set-google-translate-languages nil))

            ;;       (defun google-translate-to-korean (&optional str)
            ;;         "Translate given string automatically without language selection prompt."
            ;;         (let ((lang (cond
            ;;                      ((string-match "[가-힣]" str)
            ;;                       "ko")
            ;;                      ((or (string-match "[ァ-ヶー]" str)
            ;;                           (string-match "[ぁ-んー]" str)
            ;;                           ;; (string-match "[亜-瑤]" str)
            ;;                           )
            ;;                       "ja")
            ;;                      ((string-match "[一-龥]" str)
            ;;                       "zh-CN")
            ;;                      (t
            ;;                       "en"))))
            ;;           (google-translate-translate lang
            ;;                                       (if (string= "ko" lang) "en" "ko")
            ;;                                       str)))

            ;;       (defun korean/popup-translation (&optional str)
            ;;         "Display Google translation in tooltip."
            ;;         (interactive)
            ;;         (let* ((str (cond ((stringp str) str)
            ;;                           (current-prefix-arg
            ;;                            (read-string "Google Translate: "))
            ;;                           ((use-region-p)
            ;;                            (buffer-substring (region-beginning) (region-end)))
            ;;                           (t
            ;;                            (save-excursion
            ;;                              (let (s)
            ;;                                (forward-char 1)
            ;;                                (backward-sentence)
            ;;                                (setq s (point))
            ;;                                (forward-sentence)
            ;;                                (buffer-substring s (point)))))))
            ;;                (translated-str (save-window-excursion
            ;;                                  (funcall 'google-translate-to-korean
            ;;                                           (replace-regexp-in-string "^\\s-+" str))
            ;;                                  (switch-to-buffer "*Google Translate*")
            ;;                                  (buffer-string))))
            ;;           (if (region-active-p)
            ;;               (run-at-time 0.1 nil 'deactivate-mark))
            ;;           (kill-buffer "*Google Translate*")
            ;;           (popup-tip translated-str
            ;;                      :point (point)
            ;;                      :around t
            ;;                      ;; :height 30
            ;;                      :scroll-bar t
            ;;                      :margin t)))

            ;;       ;; (evil-leader/set-key "xgg" 'korean/popup-translation)

            ;;       ;; (spacemacs/set-leader-keys
            ;;       ;;   "xgL" 'spacemacs/set-google-translate-languages
            ;;       ;;   "xgl" 'spacemacs/set-google-translate-target-language
            ;;       ;;   "xgi" 'google-translate-paragraphs-insert
            ;;       ;;   "xgo" 'google-translate-paragraphs-overlay
            ;;       ;;   "xgQ" 'google-translate-query-translate-reverse
            ;;       ;;   "xgq" 'google-translate-query-translate
            ;;       ;;   "xgT" 'google-translate-at-point-reverse
            ;;       ;;   "xgt" 'google-translate-at-point)
            ;;       ;; (setq google-translate-enable-ido-completion nil)
            ;;       (setq google-translate-show-phonetic t)
            ;;       (setq google-translate-default-source-language "auto"
            ;;             google-translate-default-target-language "ko")
            ;;       )
            ;;     )
            ;;   )

            ;;; packages.el ends here

            ```

    <!--list-separator-->

    8.  Writing Tools



        <!--list-separator-->

        1.  Palimpsest



            ```elisp

            ;;;; Writing Tools
            ;;;;; Palimpsest
            (defun jh-writing/init-palimpsest ()
              (use-package palimpsest
                :after org
                :defer 6
                :hook (org-mode . palimpsest-mode)
                ;; (add-hook 'text-mode-hook 'palimpsest-mode)

                ;; M-x palimpsest-move-region-to-bottom
                ;; M-x palimpsest-move-region-to-top
                ;; M-x palimpsest-move-region-to-trash

                ;; Keyboard shortcuts are provided:
                ;; C-c C-r: Send selected text to bottom of buffer
                ;; C-c C-s: Send selected text to top of buffer
                ;; C-c C-q: Send selected text to trash file
                )
              )
            ```

        <!--list-separator-->

        2.  Focus-mode



            ```elisp

            ;;;;; Focus-mode

            (defun jh-writing/init-olivetti ()
              (use-package olivetti
                :custom
                ;; (olivetti-body-width 0.7) ; nil
                (olivetti-minimum-body-width 90) ; for compatibility fill-column 80
                (olivetti-recall-visual-line-mode-entry-state t))
              )

            (defun jh-writing/init-logos ()
              (use-package logos
                :after olivetti
                :init
                ;; If you want to use outlines instead of page breaks (the ^L):
                (setq logos-outlines-are-pages t)
                ;; This is the default value for the outlines:
                (setq logos-outline-regexp-alist
                      `((emacs-lisp-mode . "^;;;+ ")
                        (org-mode . "^\\*+ +")
                        (markdown-mode . "^\\#+ +")
                        (t . ,(if (boundp 'outline-regexp) outline-regexp logos--page-delimiter))))

                ;; These apply when `logos-focus-mode' is enabled.  Their value is
                ;; buffer-local.
                (setq-default logos-hide-cursor nil)
                (setq-default logos-hide-mode-line nil)
                (setq-default logos-hide-buffer-boundaries t)
                (setq-default logos-hide-fringe t)
                (setq-default logos-variable-pitch nil) ; see my `fontaine' configurations
                (setq-default logos-buffer-read-only nil)
                (setq-default logos-scroll-lock nil)
                (setq-default logos-olivetti t)

                :config
                ;; I don't need to do `with-eval-after-load' for the `modus-themes' as
                ;; I always load them before other relevant potentially packages.
                (add-hook 'modus-themes-after-load-theme-hook #'logos-update-fringe-in-buffers)

                (let ((map global-map))
                  (define-key map [remap narrow-to-region] #'logos-narrow-dwim)
                  (define-key map [remap forward-page] #'logos-forward-page-dwim)
                  (define-key map [remap backward-page] #'logos-backward-page-dwim)
                  (define-key map (kbd "M-]") #'logos-forward-page-dwim)
                  (define-key map (kbd "M-[") #'logos-backward-page-dwim)
                  )

                ;; (defun my/logos-presentation-toggle ()
                ;;   (interactive)
                ;;   (if (eq logos-focus-mode t)
                ;;       (my/logos-presentation-off)
                ;;     (my/logos-presentation-on)))

                ;; (defun my/logos-presentation-on ()
                ;;   (interactive)
                ;;   (setq-local  org-hide-emphasis-markers t)
                ;;   ;; (call-interactively 'logos-narrow-dwim)
                ;;   (olivetti-mode t)

                ;;   (org-overview)
                ;;   ;; (org-show-entry)
                ;;   ;; (org-show-children)
                ;;   (org-hide-properties)

                ;;   (setq-local header-line-format nil)

                ;;   (spacemacs/toggle-mode-line-off)
                ;;   (spacemacs/toggle-fill-column-indicator-off)
                ;;   (spacemacs/toggle-line-numbers-off)

                ;;   (tab-bar-rename-tab "PRESENTATION" 1)

                ;;   (fontaine-set-preset 'presentation)
                ;;   (git-gutter-mode -1)
                ;;   (sideline-mode -1)
                ;;   )

                ;; (defun my/logos-presentation-off ()
                ;;   (interactive)
                ;;   ;; (call-interactively 'widen)
                ;;   (olivetti-mode -1)

                ;;   (org-show-properties)
                ;;   ;; (setq-local  org-hide-emphasis-markers nil)
                ;;   (spacemacs/toggle-mode-line-on)
                ;;   (spacemacs/toggle-fill-column-indicator-on)
                ;;   (spacemacs/toggle-line-numbers-on)
                ;;   ;; (display-line-numbers-mode t)

                ;;   ;; (fontaine-restore-latest-preset) ; restore
                ;;   (tab-bar-rename-tab "WORK-SPACE" 1)

                ;;   ;; (setq-local face-remapping-alist '((default variable-pitch default)))

                ;;   (fontaine-set-preset 'regular)
                ;;   (git-gutter-mode t)
                ;;   (sideline-mode t)
                ;;   )
                ;; (spacemacs/set-leader-keys-for-major-mode 'org-mode
                ;;   "T1" 'my/logos-presentation-on
                ;;   "T2" 'my/logos-presentation-off)

                ;; place point at the top when changing pages, but not in `prog-mode'
                (defun prot/logos--recenter-top ()
                  "Use `recenter' to reposition the view at the top."
                  (unless (derived-mode-p 'prog-mode)
                    (recenter 1))) ; Use 0 for the absolute top
                (add-hook 'logos-page-motion-hook #'prot/logos--recenter-top)

                ;; Also consider adding keys to `logos-focus-mode-map'.  They will take
                ;; effect when `logos-focus-mode' is enabled.
                ;; Make EWW look like the rest of Emacs
                (setq shr-max-width fill-column)
                (setq shr-use-fonts nil)
                )
              )
            ```

        <!--list-separator-->

        3.  separedit



            ```elisp

            ;;;;; separedit

            (defun jh-writing/init-separedit ()
              (use-package separedit
                :ensure t
                ;; Key binding for modes you want edit
                ;; or simply bind ?global-map? for all.
                :bind (
                       :map prog-mode-map
                       ("C-c '" . separedit)
                       :map minibuffer-local-map
                       ("C-c '" . separedit)
                       :map help-mode-map
                       ("C-c '" . separedit))
                :init
                ;; Default major-mode for edit buffer
                ;; can also be other mode e.g. ?org-mode?.
                ;; (setq separedit-default-mode 'markdown-mode)

                ;; Feature options
                (setq separedit-preserve-string-indentation t)
                (setq separedit-continue-fill-column t)
                ;; (setq separedit-write-file-when-execute-save t)
                ;; (setq separedit-remove-trailing-spaces-in-comment t)
                )
              )
            ```

        <!--list-separator-->

        4.  Binder



            ```elisp

            ;;;;; Binder

            (defun jh-writing/init-binder ()
              (use-package binder
                :if (not (or my/remote-server *is-termux*))
                :defer 8
                :config
                (require 'binder-tutorial)  ;; optional
                )
              )
            ```


#### <span class="section-num">3.7.3</span> The `jh-writing` funcs.el {#h:0f9708e3-a336-45d0-83e2-9dc690754acd}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

(defun my/define-word--convert-html-tag-to-face (str)
  "Replace semantical HTML markup in STR with the relevant faces."
  (with-temp-buffer
    (insert str)
    ;; Remove empty <i></i> tags
    (goto-char (point-min))
    (while (re-search-forward "<i></i>" nil t)
      (replace-match ""))
    (cl-loop for (regexp face) in define-word--tag-faces do
             (define-word--regexp-to-face regexp face))
    (buffer-string)))

;;;###autoload
(defun my/compleseus-search-dir ()
  "Search current folder with no initial input"
  (interactive)
  (spacemacs/compleseus-search nil default-directory))

(defun my/compleseus-search-auto-hidden ()
  "Search folder with hiddens files"
  (interactive)
  (let*
      ((initial-directory (read-directory-name "Start from directory: "))
       (consult-ripgrep-args
        (concat "rg "
                "--null "
                "-. "  ;; for dotfiles e.g. .spacemacs.el
                "--line-buffered "
                "--color=never "
                "--line-number "
                "--smart-case "
                "--no-heading "
                "--max-columns=1000 "
                "--max-columns-preview "
                "--with-filename "
                (shell-quote-argument initial-directory))))
    (consult-ripgrep)))

;;; logos-focus

(defun my/logos-focus-editing-toggle ()
  (interactive)
  (if (eq logos-focus-mode t)
      (my/logos-focus-editing-off)
    (my/logos-focus-editing-on)))

(defun my/logos-focus-editing-on ()
  (git-gutter-mode -1)
  ;; (centered-cursor-mode t)
  (logos-focus-mode t)
  )

(defun my/logos-focus-editing-off ()
  (logos-focus-mode -1)
  ;; (centered-cursor-mode -1)
  (git-gutter-mode 1)
  )

;;; funcs.el ends here
```


#### <span class="section-num">3.7.4</span> The `jh-writing` keybindings.el {#h:eb7cb098-69a5-47d0-a082-ba4010d18d7e}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; EVIL

;; | ~C-o~       | evil-jump-backward |
;; | ~C-i~       | evil-jump-forward  |

(global-set-key (kbd "<mouse-8>") 'evil-jump-backward)
(global-set-key (kbd "<mouse-9>") 'evil-jump-forward)


;; Map extra mouse buttons to jump forward/backward
;; (global-set-key (kbd "<mouse-8>") 'better-jumper-jump-backward)
;; (global-set-key (kbd "<mouse-9>") 'better-jumper-jump-forward)

;;;; Multiple-Cursors

;; Idea taken from "Emacs: Define Key Sequence"
;; ref: http://ergoemacs.org/emacs/emacs_keybinding_power_of_keys_sequence.html
;; define prefix keymap for multiple cursors
(define-prefix-command 'my-multi-cursor-keymap)
(define-key my-multi-cursor-keymap (kbd "e") 'mc/edit-lines)
(define-key my-multi-cursor-keymap (kbd "a") 'mc/mark-all-like-this-dwim)
(define-key my-multi-cursor-keymap (kbd "r") 'mc/mark-all-in-region-regexp)
(define-key my-multi-cursor-keymap (kbd "s") 'mc/mark-all-symbols-like-this-in-defun)
(define-key my-multi-cursor-keymap (kbd "w") 'mc/mark-all-words-like-this-in-defun)
(define-key my-multi-cursor-keymap (kbd "C-n") 'mc/mark-next-like-this)
(define-key my-multi-cursor-keymap (kbd "C-p") 'mc/mark-previous-like-this)
(define-key my-multi-cursor-keymap (kbd "C-a") 'mc/mark-all-like-this)
(define-key global-map (kbd "M-c m") 'my-multi-cursor-keymap)

;; from ahyatt-dotfiles
;; phi-search : another incremental search & replace, compatible with "multiple-cursors"
(global-set-key (kbd "M-s-r") 'mc/mark-all-like-this-dwim)
(global-set-key (kbd "M-C-s") 'phi-search) ;; conflict isearch-regex
(global-set-key (kbd "M-C-r") 'phi-search-backward)

;;; Editing

;; Info 모드 Node 이동
;; (evil-define-key '(motion normal visual) Info-mode-map
;;   "^" 'Info-up
;;   "C-n" 'Info-prev
;;   "C-p" 'Info-next
;;   "M-n" 'Info-forward-node
;;   "M-p" 'Info-backward-node
;;   )

;; use -- (kbd "C-x x v") 'view-text-file-as-info-manual
(add-hook 'Info-mode-hook
          (lambda ()
            (define-key Info-mode-map (kbd "M-[") 'Info-history-back)
            (define-key Info-mode-map (kbd "M-]") 'Info-history-forward)))
;;             (define-key Info-mode-map (kbd "<mouse-5>") 'Info-history-forward)
;;             (define-key Info-mode-map (kbd "<mouse-4>") 'Info-history-back)

;; o :: ace-link-info 이거면 충분하다.
;; [[file:~/spacemacs/doc/DOCUMENTATION.org::*Binding keys][Binding keys]]
(define-key evil-insert-state-map (kbd "C-]") 'forward-char)

;; Replace Emacs Tabs key bindings with Workspace key bindings
(with-eval-after-load 'evil-maps

  ;; replace "." search with consul-line in Evil normal state
  ;; use default "/" evil search
  (evil-global-set-key 'normal "." 'consult-line)

  ;; TODO disable evil-mc key-binding with meta
  ;; 내 커스텀 키로 이용한다. evil-mc 를 어떻게 할지 고민
  ;; (with-eval-after-load 'evil-mc
  ;;   (evil-define-key '(insert normal visual) evil-mc-key-map (kbd "M-n") nil)
  ;;   (evil-define-key '(insert normal visual) evil-mc-key-map (kbd "M-p") nil)
  ;;   (evil-define-key '(normal visual) evil-mc-key-map (kbd "C-n") nil)
  ;;   (evil-define-key '(normal visual) evil-mc-key-map (kbd "C-p") nil)
  ;;   (evil-define-key '(normal visual) evil-mc-key-map (kbd "C-t") nil)
  ;;   (evil-define-key '(normal visual) evil-mc-key-map (kbd "C-M-j") nil)
  ;;   (evil-define-key '(normal visual) evil-mc-key-map (kbd "C-M-k") nil)
  ;; )

  (define-key evil-normal-state-map (kbd "C-a") 'evil-beginning-of-line)
  (define-key evil-normal-state-map (kbd "C-e") 'evil-end-of-line-or-visual-line)
  (define-key evil-insert-state-map (kbd "C-a") 'evil-beginning-of-line)
  (define-key evil-insert-state-map (kbd "C-e") 'evil-end-of-line-or-visual-line)
  ;; =C-w= 'insert 'evil-delete-backward-word
  ;; =C-w= 'visual 'evil-window-map
  )


;; (define-key puni-mode-map (kbd "<deletechar>") 'eli/puni-hungry-backward-delete-char)

(global-set-key (kbd "C-M-u") 'universal-argument)

;; (spacemacs/set-leader-keys "jd" 'xref-find-definitions)
;; (spacemacs/set-leader-keys "jD" 'xref-pop-marker-stack)
;; (spacemacs/set-leader-keys "j?" 'xref-find-references)

(global-set-key (kbd "C-c v r") 'vr/replace)
(global-set-key (kbd "C-c v q") 'vr/query-replace)

;; (global-set-key (kbd "M-s g") 'consult-grep)
;; (global-set-key (kbd "M-s r") 'consult-ripgrep)
(global-set-key (kbd "M-s r") 'deadgrep)
(global-set-key (kbd "M-s F") 'my/compleseus-search-auto-hidden)
(global-set-key (kbd "M-s M-r") 'rg-menu)

(spacemacs/set-leader-keys "sg" 'consult-grep)
(spacemacs/set-leader-keys "sk" 'consult-keep-lines)

(spacemacs/set-leader-keys "ss" 'consult-line)

(spacemacs/set-leader-keys "sd" #'my/compleseus-search-dir)
(spacemacs/set-leader-keys "sD" #'spacemacs/compleseus-search-dir)
(spacemacs/set-leader-keys "sF" 'my/compleseus-search-auto-hidden)

(spacemacs/set-leader-keys "s M-r" 'rg-menu)
(spacemacs/set-leader-keys "sr" 'deadgrep)

(global-set-key (kbd "M-s a") 'affe-grep)
(global-set-key (kbd "M-s A") 'affe-find)
(spacemacs/set-leader-keys "sa" 'affe-grep)
(spacemacs/set-leader-keys "sA" 'affe-find)

;;; Structural Editing

;; /corgi-packages/corgi-bindings/corgi-keys.el:111

;; normal visual
(evil-define-key '(normal visual) prog-mode-map (kbd ">") 'puni-slurp-forward)
(evil-define-key '(normal visual) prog-mode-map (kbd "<") 'puni-barf-forward)

;; The latter two are "text objects" that can be combined with other vim-style operations
;; - ~yL~ copy next sexp (paste with ~p~
;; - ~dL~ delete next sexp
;; - ~cL~ "change" sexp (delete and switch to insert mode)
(evil-define-key '(normal visual) prog-mode-map (kbd "L") 'puni-forward-sexp)
(evil-define-key '(normal visual) prog-mode-map (kbd "H") 'puni-backward-sexp)

(evil-define-key '(normal visual) prog-mode-map (kbd "M-l") 'puni-end-of-sexp)
(evil-define-key '(normal visual) prog-mode-map (kbd "M-h") 'puni-beginning-of-sexp)

(spacemacs/set-leader-keys
  "xs" #'puni-splice-killing-backward
  ;; "xT" #'transpose-sexps
  "xT" #'puni-transpose
  "xdd" #'delete-pair
  "xD" #'delete-pair
  "c;" #'pp-eval-expression
  ;; "fek" #'my/open-usr-key-file
  "js" #'puni-split
  "jj" #'avy-goto-char
  ;; "jD" nil
  ;; "jJ" 'avy-goto-char-timer
  "jn" #'electric-newline-and-maybe-indent
  "fel" #'find-library
  )

(spacemacs/declare-prefix
  "xs"   "⋇splice-backward-sexp"
  "xT"   "⋇transpose-sexps"
  "xD"   "⋇delete-pair"
  "c;"   "⋇pp-eval-expr"
  "ji"   "⋇jump/identifier"
  "jj"   "⋇jump/character"
  "jc"   "⋇jump/last-change"
  "jn"   "⋇jump/newline-indent"
  )

;;; Markdown

(spacemacs/set-leader-keys-for-major-mode 'markdown-mode
  "/" 'math-preview-at-point
  "M-/" 'math-preview-all
  "C-M-/" 'math-preview-clear-all
  )

;;; Expand-region

(global-set-key (kbd "C-=") 'eli/expand-region)

(evil-define-key '(normal visual) prog-mode-map (kbd "M-<up>") 'er/expand-region)
(evil-define-key '(normal visual) prog-mode-map (kbd "M-<down>") 'er/contract-region)
(evil-define-key '(normal visual) org-mode-map (kbd "M-<up>") 'er/expand-region)
(evil-define-key '(normal visual) org-mode-map (kbd "M-<down>") 'er/contract-region)
(evil-define-key '(normal visual) markdown-mode-map (kbd "M-<up>") 'er/expand-region)
(evil-define-key '(normal visual) markdown-mode-map (kbd "M-<down>") 'er/contract-region)

;;; Translation

;; 'C-c t' translate-map
;; (define-prefix-command 'my-translate-map)
;; (define-key global-map (kbd "C-c t") 'my-translate-map)
;; (let ((map my-translate-map))
;;   (define-key map (kbd "t") 'txl-translate-insert)
;;   (define-key map (kbd "i") 'txl-translate-insert)
;;   (define-key map (kbd "p") 'txl-translate-region-or-paragraph)
;;   (define-key map (kbd "g") 'google-translate-paragraphs-insert)
;;   (define-key map (kbd "d") 'gts-do-translate)
;;   )

(spacemacs/set-leader-keys "xg" nil)
(spacemacs/declare-prefix "xg" "translation")
(spacemacs/set-leader-keys "xgi" #'txl-translate-insert)
(spacemacs/set-leader-keys "xgp" #'txl-translate-region-or-paragraph)
(spacemacs/set-leader-keys "xgg" #'google-translate-paragraphs-insert)

;; "xgo" 'google-translate-paragraphs-overlay
;; "xgQ" 'google-translate-query-translate-reverse
;; "xgq" 'google-translate-query-translate
;; "xgT" 'google-translate-at-point-reverse
;; "xgt" 'google-translate-at-point

;;; Dictionary

;; M-c
(global-set-key (kbd "M-c i") 'txl-translate-insert)
(global-set-key (kbd "M-c d") 'gts-do-translate)
(global-set-key (kbd "M-c w") 'wiki-summary-insert)
(global-set-key (kbd "M-c p") 'immersive-translate-paragraph)
(global-set-key (kbd "M-c a") 'immersive-translate-auto-mode)

(global-set-key (kbd "M-c /") 'sdcv-search-pointer)
(global-set-key (kbd "M-c ?") 'sdcv-search-input)
(global-set-key (kbd "M-c .") 'sdcv-search-pointer+) ; posframe
(global-set-key (kbd "M-c ,") 'my/wr-enko)

(global-set-key (kbd "M-c b") 'binder-toggle-sidebar)

(spacemacs/defer-until-after-user-config  ; otherwise, spacemacs-default layer would override the binding
 (lambda ()                               ; and set it to `duplicate-line-or-region', and it's pretty useles for me
   (spacemacs/set-leader-keys "xwi" #'define-it-at-point)
   (spacemacs/set-leader-keys "xwb" #'wiktionary-bro-dwim)
   (spacemacs/set-leader-keys "xww" #'define-word-at-point)
   (spacemacs/set-leader-keys "xwl" #'lexic-search)
   (spacemacs/set-leader-keys "xwe" #'external-dict-dwim)
   (spacemacs/set-leader-keys "xwm" #'mw-thesaurus-lookup-dwim)
   (spacemacs/set-leader-keys "xwp" #'powerthesaurus-transient)
   (spacemacs/set-leader-keys "xwd" #'dictionary-search)
   (spacemacs/set-leader-keys "xws" #'sdcv-search-pointer)
   (spacemacs/set-leader-keys "xwk" #'my/wr-enko)
   ))

;; 'C-c d'
(global-set-key (kbd "C-c d b") 'wiktionary-bro-dwim)
(global-set-key (kbd "C-c d i") 'define-it-at-point)
(global-set-key (kbd "C-c d w") 'define-word-at-point)
(global-set-key (kbd "C-c d l") 'lexic-search)
(global-set-key (kbd "C-c d s") 'sdcv-search-pointer+)

(global-set-key (kbd "C-c d e") 'external-dict-dwim)
(global-set-key (kbd "C-c d m") 'mw-thesaurus-lookup-dwim)
(global-set-key (kbd "C-c d p") 'powerthesaurus-transient)
(global-set-key (kbd "C-c d P") 'powerthesaurus-hydra/body)
(global-set-key (kbd "C-c d d") 'dictionary-search)

;;; find-files-in-project

(spacemacs/declare-prefix "sP"  "find-file-in-project")
(spacemacs/set-leader-keys "sPc" 'find-file-in-current-directory-by-selected)
(spacemacs/set-leader-keys "sPp" 'find-file-in-project-by-selected)

;;; DONT Obsidian

;; 'C-c n' my-obsidian-map
;; (define-prefix-command 'my-obsidian-map)
;; (define-key global-map (kbd "C-c o") 'my-obsidian-map)
;; (define-key global-map (kbd "C-c O") 'obsidian-hydra/body)

;; ;; 2023-05-28 project, agenda 관련 부분은 obsidian 에서 제거한다.
;; (let ((map my-obsidian-map))
;;   (define-key map (kbd "o") 'obsidian-follow-wiki-link-at-point)
;;   (define-key map (kbd "b") 'obsidian-backlink-jump)
;;   (define-key map (kbd "l") 'obsidian-insert-wikilink)
;;   (define-key map (kbd "h") 'obsidian-hydra/body)
;;   )

;;; keybindings.el ends here
```


### <span class="section-num">3.8</span> The `jh-reading` layer {#h:72dd1af6-f31d-4167-a0d2-b466ee148bc3}




#### <span class="section-num">3.8.1</span> The `jh-reading` layer.el {#h:192cdc51-8e93-473a-a653-3442175c9eea}



```elisp

;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   (elfeed
    :variables
    elfeed-enable-goodies t
    rmh-elfeed-org-files (list "~/sync/org/elfeed/elfeed.org")

    ;; (rmh-elfeed-org-files
    ;;  (denote-directory-files-matching-regexp "elfeed"))

    ;; elfeed-db-directory "~/.spacemacs.d/.elfeed"
    elfeed-enable-web-interface nil
    elfeed-goodies/show-mode-padding 2
    elfeed-goodies/powerline-default-separator 'slant
    elfeed-goodies/feed-source-column-width 25
    elfeed-goodies/tag-column-width 30
    url-queue-timeout 30)

   epub

   ;; eaf

   (eww
    :packages (not texfrag)
    :variables
    shr-max-image-proportion 0.6
    shr-width fill-column          ; check `prot-eww-readable'
    shr-max-width fill-column
    shr-use-fonts nil)

   ;; xkcd

   ;; fast, global-search and tag-based email system
   ;; (notmuch
   ;;  :variables
   ;;  notmuch-messages-deleted-tags '("+deleted" "-inbox" "-unread"))

   (pocket :variables
           pocket-reader-color-title nil
           pocket-reader-color-site t
           pocket-reader-site-column-max-width 16)

   pdf

   ;; dash ; use consult-dash

   speed-reading
   ))
```


#### <span class="section-num">3.8.2</span> The `jh-reading` packages.el {#h:3840609c-c194-4197-9c3f-84446bf1a82e}

<!--list-separator-->

1.  Packages



    ```elisp

    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-reading-packages
      '(
        ;; 재정의 패키지
        ;; eww
        nov
        ;; 새로 등록하는 패키지
        subed

        (hypothesis :location (recipe :fetcher github :repo "EFLS/hypothesis"))

        ;; ement ;; matrix client

        tldr ; read manual

        ;; consult-notmuch

        emms

        sicp      ; sicp info-mode

        youtube-sub-extractor

        howdoyou
        (google-suggest :location (recipe
                                   :fetcher github
                                   :repo "thanhvg/emacs-google-suggest"))

        ;; mastodon ; ActivityPub protocol
        ;; elfeed-webkit

        ;; elfeed-tube
        ;; elfeed-tube-mpv

        ;; (empv :location (recipe :fetcher github
        ;;                 :repo "isamert/empv.el"))

        ;; (youtube-dl :location (recipe :fetcher github :repo "skeeto/youtube-dl-emacs"))
        ))

    ```

<!--list-separator-->

2.  Configurations

    ```elisp


    ;;;; nov :  reading epub books

    (defun jh-reading/post-init-nov()
      (add-hook 'nov-mode-hook (lambda ()
                                 (visual-line-mode)
                                 (setq line-spacing 0.5)))
      (add-hook 'nov-mode-hook #'olivetti-mode)
      ;; (require 'olivetti)
      ;; (setq nov-text-width (- olivetti-body-width 2))
      )

    ;; (defun jh-reading/post-init-elfeed ()
    ;;   (use-package elfeed
    ;;     :bind (:map elfeed-search-mode-map
    ;;                 ("h" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +hack")))
    ;;                 ("u" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +understand")))
    ;;                 ("e" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +experience")))
    ;;                 ("f" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +fp")))
    ;;                 ("l" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +listen")))
    ;;                 ("w" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread +watch")))
    ;;                 ("A" . (lambda () (interactive)
    ;;                          (elfeed-search-set-filter "@1-months-ago +unread")))
    ;;                 ("q" . deb/elfeed-save-bury)))
    ;;   )

    ;;;; mpv


    ;;;; Elfeed-tube

    ;; Improve Elfeed Youtube RSS Feeds Integration
    ;; (defun jh-reading/init-elfeed-tube ()
    ;;   (use-package elfeed-tube
    ;;    :after elfeed
    ;;    :defer 9
    ;;    :config
    ;;    ;; (setq elfeed-tube-auto-save-p nil) ; default value
    ;;    (setq elfeed-tube-auto-fetch-p t)  ; default value
    ;;    (setq elfeed-tube-captions-languages
    ;;        '("en" "english (auto generated)"))
    ;;    (elfeed-tube-setup)

    ;;    :bind (:map elfeed-show-mode-map
    ;;            ("F" . elfeed-tube-fetch)
    ;;            ([remap save-buffer] . elfeed-tube-save)
    ;;            :map elfeed-search-mode-map
    ;;            ("F" . elfeed-tube-fetch)
    ;;            ([remap save-buffer] . elfeed-tube-save)
    ;;            )
    ;;    )
    ;;   )

    ;; (defun jh-reading/init-elfeed-tube-mpv ()
    ;;   (use-package elfeed-tube-mpv
    ;;    :after elfeed
    ;;    :defer 20
    ;;    :bind (:map elfeed-show-mode-map
    ;;            ("C-c C-f" . elfeed-tube-mpv-follow-mode)
    ;;            ("C-c C-w" . elfeed-tube-mpv-where))
    ;;    )
    ;;   )

    ;;;; Youtube-sub-extractor

    (defun jh-reading/init-youtube-sub-extractor ()
      (use-package youtube-sub-extractor
        :commands (youtube-sub-extractor-extract-subs)
        :config
        ;; (map! :map youtube-sub-extractor-subtitles-mode-map
        ;;   :desc "copy timestamp URL" :n "RET" #'youtube-sub-extractor-copy-ts-link
        ;;   :desc "browse at timestamp" :n "C-c C-o" #'youtube-sub-extractor-browse-ts-link)
        ;; (setq youtube-sub-extractor-min-chunk-size 7) ;; default
        (setq youtube-sub-extractor-timestamps 'left-margin) ; or nil
        )
      )

    ;;;; emms

    (defun jh-reading/init-emms ()
      (use-package emms))

    ;; (+map! :infix "o"
    ;;   "v"  '(nil :wk "empv")
    ;;   "vp" '(empv-play :wk "Play")
    ;;   "vy" '(consult-empv-youtube :wk "Seach Youtube")
    ;;   "vr" '(empv-play-radio :wk "Play radio")
    ;;   "vs" '(empv-playtlist-save-to-file :wk "Save current playlist")
    ;;   "vD" '(+empv-download-playtlist-files :wk "Download current's playlist files")

    ;;;; empv

    ;; (defun jh-reading/init-empv ()
    ;;   (use-package empv
    ;;    :after emms embark
    ;;    :when MPV-P
    ;;    :defer 15
    ;;    :config
    ;;    (require 'emms)

    ;;    (defun +browse-url-mpv (url &optional args)
    ;;      "Open URL with MPV."
    ;;      (start-process "mpv" " *MPV*" "mpv" url))

    ;;    ;; Automatically open Youtube links in MPV
    ;;    (setq browse-url-handlers
    ;;        `((,(rx (seq "http" (? ?s) "://" (? "www.") (or "youtube.com" "youtu.be"))) . +browse-url-mpv)
    ;;          ("." . browse-url-default-browser)))

    ;;    ;; See https://docs.invidious.io/instances/
    ;;    ;; https://yt.funami.tech/feed/playlists
    ;;    (setq empv-invidious-instance "https://vid.puffyan.us/api/v1")
    ;;    ;; (setq empv-invidious-instance "https://yt.funami.tech/api/v1")
    ;;    ;; (setq empv-invidious-instance "https://invidious.projectsegfau.lt/api/v1")
    ;;    (setq empv-audio-dir "~/empv/Music")
    ;;    (setq empv-video-dir "~/empv/Videos")
    ;;    (setq empv-max-directory-search-depth 4)
    ;;    (setq empv-radio-log-file (expand-file-name "logged-radio-songs.org" org-directory))
    ;;    (setq empv-audio-file-extensions '("webm" "mp3" "ogg" "wav" "m4a" "flac" "aac" "opus"))

    ;;    ;; (add-to-list 'empv-mpv-args "--ytdl-format=best")
    ;;    (bind-key "C-x M" empv-map)

    ;;    (with-eval-after-load 'embark (empv-embark-initialize-extra-actions))

    ;;    (spacemacs/set-leader-keys "amp" 'empv-play)
    ;;    ;; (spacemacs/set-leader-keys "amy" 'consult-empv-youtube)
    ;;    (spacemacs/set-leader-keys "amr" 'empv-play-radio)
    ;;    (spacemacs/set-leader-keys "ams" 'empv-playlist-save-to-file)
    ;;    (spacemacs/set-leader-keys "amD" '+empv-download-playtlist-files)

    ;;    (defun +empv--dl-playlist (playlist &optional dist)
    ;;      (let ((default-directory
    ;;           (or dist
    ;;              (let ((d (expand-file-name "empv-downloads" empv-audio-dir)))
    ;;                (unless (file-directory-p d) (mkdir d t)) d)))
    ;;          (vids (seq-filter
    ;;               #'identity ;; Filter nils
    ;;               (mapcar
    ;;                (lambda (item)
    ;;                  (when-let
    ;;                     ((vid (when (string-match
    ;;                              (rx (seq "watch?v=" (group-n 1 (one-or-more (or alnum "_" "-")))))
    ;;                              item)
    ;;                          (match-string 1 item))))
    ;;                    vid))
    ;;                playlist)))
    ;;          (proc-name "empv-yt-dlp"))
    ;;        (unless (zerop (length vids))
    ;;          (message "Downloading %d songs to %s" (length vids) default-directory)
    ;;          (when (get-process proc-name)
    ;;            (kill-process proc-name))
    ;;          (make-process :name proc-name
    ;;                    :buffer (format "*%s*" proc-name)
    ;;                    :command (append
    ;;                           (list
    ;;                            (executable-find "yt-dlp")
    ;;                            "--no-abort-on-error"
    ;;                            "--no-colors"
    ;;                            "--extract-audio"
    ;;                            "--no-progress"
    ;;                            "-f" "bestaudio")
    ;;                           vids)
    ;;                    :sentinel (lambda (prc event)
    ;;                            (when (string= event "finished\n")
    ;;                              (message "Finished downloading playlist files!")))))))

    ;;    (defun +empv-download-playtlist-files (&optional path)
    ;;      (interactive "DSave download playlist files to: ")
    ;;      (empv--playlist-apply #'+empv--dl-playlist path))
    ;;    )
    ;;   )

    ;;;; Wikinfo / SICP / TLDR

    (defun jh-reading/init-sicp () (use-package sicp :defer t))

    (defun jh-reading/init-tldr ()
      (use-package tldr
        :defer 10
        :config
        (spacemacs/set-leader-keys "h," 'tldr)
        :custom
        (tldr-enabled-categories '("common" "linux"))))

    ;;;; elfeed-summary

    ;; (defun jh-reading/init-elfeed-summary ()
    ;;   )

    ;;;; subed

    (defun jh-reading/init-subed ()
      (use-package subed
        :defer 9
        :config
        ;; Remember cursor position between sessions
        (add-hook 'subed-mode-hook 'save-place-local-mode)
        ;; Break lines automatically while typing
        (add-hook 'subed-mode-hook 'turn-on-auto-fill)
        ;; Break lines at 40 characters
        (add-hook 'subed-mode-hook (lambda () (setq-local fill-column 40)))
        ;; Some reasonable defaults
        (add-hook 'subed-mode-hook 'subed-enable-pause-while-typing)
        ;; As the player moves, update the point to show the current subtitle
        (add-hook 'subed-mode-hook 'subed-enable-sync-point-to-player)
        ;; As your point moves in Emacs, update the player to start at the current subtitle
        (add-hook 'subed-mode-hook 'subed-enable-sync-player-to-point)
        ;; Replay subtitles as you adjust their start or stop time with M-[, M-], M-{, or M-}
        (add-hook 'subed-mode-hook 'subed-enable-replay-adjusted-subtitle)
        ;; Loop over subtitles
        (add-hook 'subed-mode-hook 'subed-enable-loop-over-current-subtitle)
        ;; Show characters per second
        (add-hook 'subed-mode-hook 'subed-enable-show-cps)
        )
      )

    ;;;; howdoyou

    (defun jh-reading/init-google-suggest ()
      (use-package google-suggest
        :defer 10
        :init
        (spacemacs/set-leader-keys
          "yG" #'google-eww
          "yg" #'google-google))
      )

    (defun jh-reading/init-howdoyou ()
      (use-package howdoyou
        :defer 10
        :init
        (spacemacs/set-leader-keys
          "yY" #'howdoyou-query
          "yy" #'howdoyou-with-google-suggest
          "yn" #'howdoyou-next-link
          "yr" #'howdoyou-reload-link
          "y1" #'howdoyou-go-back-to-first-link
          "yp" #'howdoyou-previous-link))
      )

    ;;;; hypothesis

    ;; M-x hypothesis-to-org downloads the 200 newest notations and inserts
    ;; them into a temporary org-mode buffer. M-x hypothesis-to-archive
    ;; imports notations into hypothesis-archive. It will import up to 200
    ;; notations but will only import notations made after the last import.
    (defun jh-reading/init-hypothesis ()
      (use-package hypothesis
        :ensure t
        :commands hypothesis-to-org hypothesis-to-archive
        :config
        (setq hypothesis-username user-hypothesis-username)
        (setq hypothesis-token user-hypothesis-token)
        )
      )

    ;;;;  DONT ement - matrix client

    ;; (defun jh-reading/init-ement ()
    ;;   (use-package ement
    ;;     :defer 3
    ;;     )
    ;;   )

    ```


#### <span class="section-num">3.8.3</span> The `jh-reading` funcs.el {#h:507cf451-d54b-4a22-bb8a-fd77f7fe7beb}

```elisp

;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; /home/junghan/sync/man/dotsamples/vanilla/khoj-dotfiles/

;; Elfeed helper functions
(defun elfeed-load ()
  "Wrapper to load the elfeed db from disk before opening"
  (interactive)
  (elfeed-db-load) ;; this was commented earlier due to strange behavior
  (elfeed)
  (elfeed-search-update :force))

(defun deb/elfeed-save-bury ()
  "Wrapper to save the elfeed db to disk before burying buffer"
  (interactive)
  (elfeed-db-save)
  (quit-window))

;; (defun elfeed-junk-filter (entry)
;;   (when (or (string-match "thestranger" (elfeed-entry-link entry))
;;             (string-match "ycombinator" (elfeed-entry-link entry)))
;;     (let ((title (elfeed-entry-title entry))
;;           (content (elfeed-deref (elfeed-entry-content entry)))
;;           (trump "\\<trump\\>")
;;           (case-fold-search t))
;;       (unless (or (string-match trump title)
;;                   (string-match trump content))
;;         (elfeed-tag entry 'junk)))))

;; (add-hook 'elfeed-new-entry-hook #'elfeed-junk-filter)

;;;; howdoyou google-suggest

(defvar my/google-suggest-search-url
  "https://encrypted.google.com/search?ie=utf-8&oe=utf-8&q=%s"
  "URL used for Google searching.
This is a format string, don't forget the `%s'.")

(defun google-eww ()
  "Build query to search with google suggest."
  (interactive)
  (let* ((query (google-suggest))
         (url (format my-google-suggest-search-url
                      (url-hexify-string query))))
    (eww url)))

(defun google-google ()
  "Build query to search with google suggest."
  (interactive)
  (let* ((query (google-suggest))
         (url (format my/google-suggest-search-url
                      (url-hexify-string query))))
    (browse-url url)))

(defun howdoyou-with-google-suggest ()
  "`howdoyou-query' with `google-suggest'."
  (interactive)
  (howdoyou-query (google-suggest)))

```


### <span class="section-num">3.9</span> The `jh-coding` layer {#h:cccaecaa-5016-48f9-99d1-48ad84baab98}




#### <span class="section-num">3.9.1</span> The `jh-coding` layer.el {#h:4a647505-bd7e-446e-a11e-0db5f1844643}



```elisp

;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;; brew install tidy-html5

;; npm i -g \
;;      typescript-language-server \
;;      typescript \
;; npm install -g eslint babel-eslint eslint-plugin-react
;; npm i -g pyright

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
;;;; Basics

   (dash :variables zeal-at-point-zeal-version "0.7.0")

   csv ; Tools to work with comma separate values e.g. data science data
   graphviz ; graphviz - open-source graph declaration system

   (json :variables
         json-fmt-on-save nil
         json-backend nil) ; not 'lsp

   (yaml :variables yaml-enable-lsp nil)

   ;; linting, style checking, formatting supports
   (shell-scripts
    :packages (not fish-mode)
    :variables
    shell-scripts-format-on-save t ; with shfmt
    shell-scripts-backend nil ; 'lsp
    )

   (html :variables web-fmt-tool 'prettier)

   prettier ; + format-all or apheleia
   debug
   docker
   dap

   ;; Language server protocol with minimal visual impact
   ;; https://practical.li/spacemacs/install-spacemacs/clojure-lsp/
   ;; https://emacs-lsp.github.io/lsp-mode/tutorials/how-to-turn-off/
   (lsp
    :packages (not helm-lsp lsp-ivy) ; lsp-treemacs lsp-origami
    )

   pandoc

   restclient

   (semantic-web :packages (not ttl-mode))

   ;; prodigy
   ;; protobuf
   ;; tmux
   ;; kubernetes
   ;;  (gtags :variables gtags-enable-by-default t)
   ;; systemd
   ;; (command-log :packages (command-log-mode)) ; never!

;;;; LISP

   (emacs-lisp
    :packages (not emr))

   ;; For common-lisp dev. work with slime
   ;; you should install latest stable 'sbcl' first
   ;; common-lisp

   ;; racket

   (clojure
    :variables
    clojure-backend 'lsp
    clojure-enable-kaocha-runner t ; Kaocha test runner
    clojure-enable-sayid nil ; default nil

    ;; https://practical.li/spacemacs/testing/unit-testing/configure-cider-test-runner/
    cider-test-show-report-on-success t
    clojure-enable-clj-refactor nil ; dependent on paredit
    )

;;;; Python

   ;; for Ubuntu
   ;; sudo apt-get install python3-full
   ;; sudo apt-get install pipx
   ;; pipx install pyright --force

   (python
    :packages (not helm-cscope nose helm-pydoc company-anaconda importmagic lsp-python-ms)
    :variables
    python-backend 'lsp
    python-lsp-server 'pyright
    python-indent-offset 4
    python-test-runner 'pytest
    python-formatter 'lsp
    python-format-on-save t
    python-poetry-activate nil
    python-sort-imports-on-save nil
    python-save-before-test nil
    )

   ;; django
   ;; (ipython-notebook :variables ein-backend 'jupyter)

;;;; Web - js-modes :: js-ts-mode typescript-ts-mode tsx-ts-mode

   (node :variables node-add-modules-path t)

   ;; import-js

   (javascript
    :variables
    js-indent-level 4
    js2-basic-offset 4
    javascript-fmt-tool 'prettier
    javascript-backend 'lsp
    javascript-repl 'nodejs
    js2-mode-show-strict-warnings nil
    javascript-lsp-linter nil ; npm install -g eslint
    node-add-modules-path t
    )

   (typescript
    :variables
    typescript-backend 'lsp
    typescript-fmt-tool 'prettier
    typescript-lsp-linter nil
    ;; typescript-fmt-on-save t
    )

   ;; react

;;;; DONT Not Now

   ;; elixir

   ;; (go :variables
   ;;     go-backend 'lsp
   ;;     go-format-before-save t
   ;;     go-tab-width 4
   ;;     )

   ;; (ruby :variables
   ;;       ruby-backend 'lsp ; default 'robe or 'lsp
   ;;       ruby-test-runner 'ruby-test ; default ruby-test
   ;;       ruby-version-manager nil ;  default nil -- rbenv, rvm, chruby
   ;;       ruby-prettier-on-save nil ; default nil
   ;;       )

   ;; (rust :variables
   ;;       rust-format-on-save t
   ;;       rust-backend 'lsp)

   ;; sql

   ;; ess

   ;; (julia :variables
   ;;        ;; julia-mode-enable-ess t ; use 'ess' layer
   ;;        julia-backend 'lsp)

;;;; TODO 15. Programming Languages
;;;;; 15.1 Domain-specific (DSLs)
;;;;; 15.2 Frameworks
;;;;; 15.3 General-purpose

   )
 )

```


#### <span class="section-num">3.9.2</span> The `jh-coding` packages.el {#h:4b3cb5db-8627-4d49-a561-d7f0a4d3780c}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;; M-x lsp-install-server 로 설치하지 말고 아래와 같이 미리 설치해 놓으라. 특히 npm 패키지류

    ;; sudo npm i -g typescript typescript-language-server
    ;; sudo npm i -g bash-language-server
    ;; sudo npm i -g yaml-language-server
    ;; sudo npm i -g vscode-css-languageserver-bin
    ;; sudo npm i -g vscode-html-languageserver-bin
    ;; sudo npm i -g vscode-langservers-extracted
    ;; sudo npm i -g pyright
    ;; sudo npm i -g eslint
    ;; sudo npm i -g import-js
    ;; sudo npm i -g vmd
    ;; sudo npm i -g typescript tslint
    ;; sudo npm i -g typescript-formatter
    ;; sudo npm i -g prettier

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-coding-packages
      '(
    ;;;;; Basics
        ;; format-all
        apheleia
        elisp-demos
        breadcrumb
        aggressive-indent

        ;; crux
        ;; topsy
        ;; scopeline

        ;; eglot
        ;; xref
        dap-mode

        lsp-mode
        lsp-ui
        lsp-treemacs
        ;; prodigy

        direnv

        (evil-textobj-tree-sitter :location (recipe :fetcher github :repo "meain/evil-textobj-tree-sitter"
                                                    :files ( "*.el" "queries" "treesit-queries")))
        (treesit-fold :location (recipe :fetcher github :repo "abougouffa/treesit-fold"))

        ;; (ts-fold :location (recipe :fetcher github :repo "garyo/ts-fold" :branch "garyo/treesit-el-patches"))
        ;; (combobulate :location (recipe :fetcher github :repo "mickeynp/combobulate"))

        ;; jsonian
        ;; consult-ag
        ;; ssh-deploy
        ;; dwim-shell-command
        ;; (unpackaged :location (recipe :fetcher github :repo "alphapapa/unpackaged"))

        ;; (deno-bridge :location (recipe :fetcher github :repo "manateelazycat/deno-bridge"
        ;;                                :files ("*.el" "*.md" "example")))
        ;; (insert-translated-name :location (recipe :fetcher github :repo "manateelazycat/insert-translated-name"
        ;;                                           :files ("*.el" "*.ts" "*.md")))

        ;; (asdf :location (recipe :fetcher github :repo "tabfugnic/asdf.el"))
    ;;;;; shell

        bats-mode

    ;;;;; LISP
        eros

    ;;;;;; Clojure
        clojure-mode
        cider

        clojars
        clojure-essential-ref-nov

        ;; parseclj
        ;; inflections
        ;; clomacs
        ;; walkclj

    ;;;;; Elixir
        ;; elixir-ts-mode
        ;; exunit

    ;;;;; Documentation

        consult-dash
        devdocs-browser ; using EWW

    ;;;;; Misc
        exercism
        leetcode

    ;;;;; ts-mode
        awk-ts-mode
    ;;;;; DONT Not Now
        ;; clojure-ts-mode
        )
      )

    ```

<!--list-separator-->

2.  Configurations

    ```elisp


    ;;;; BASICS

    ;;;;; aggressive-indent

    (defun jh-coding/init-aggressive-indent ()
      (use-package aggressive-indent
        :ensure t
        :diminish aggressive-indent-mode
        :config
        (add-hook 'emacs-lisp-mode-hook #'aggressive-indent-mode)
        (add-hook 'css-mode-hook #'aggressive-indent-mode)
        (add-hook 'js2-mode-hook #'aggressive-indent-mode)

        ;; :hook ((clojurex-mode
        ;;          clojurescript-mode
        ;;          clojurec-mode
        ;;          clojure-mode
        ;;          emacs-lisp-mode
        ;;          lisp-data-mode
        ;;          typescript-ts-mode
        ;;          tsx-ts-mode
        ;;          js-ts-mode
        ;;          piglet-mode)
        ;;         . aggressive-indent-mode)
        )
      )

    ;;;;; breadcrumb

    (defun jh-coding/init-breadcrumb ()
      (use-package breadcrumb
        :ensure t
        :init
        ;; (add-hook 'prog-mode-hook 'breadcrumb-local-mode) ; conflict with lsp-mode
        (add-hook 'emacs-lisp-mode-hook 'breadcrumb-local-mode)
        (add-hook 'markdown-mode-hook 'breadcrumb-local-mode)
        (add-hook 'org-mode-hook 'breadcrumb-local-mode)
        (add-hook 'text-mode-hook 'breadcrumb-local-mode)
        :custom
        (breadcrumb-project-max-length 0.1)
        (breadcrumb-imenu-max-length 0.2)
        )
      )

    ;;;;; elisp-demos

    (defun jh-coding/init-elisp-demos ()
      ;; also add some examples from crafter-emacs
      (use-package elisp-demos
        :after helpful
        :config
        (advice-add 'helpful-update :after #'elisp-demos-advice-helpful-update)
        )
      )
    ;;;;; DONT crux

    ;; [[https://github.com/bbatsov/crux][GitHub - bbatsov/crux: A Collection of Ridiculously Usefu...]]
    ;; 한 번에 다 볼 수 없다. 알고 있어라. Spacemacs 에 이미 있는 것들도 있다.
    ;; (defun jh-coding/init-crux ()
    ;;   (use-package crux :defer t))

    ;;;;; lsp-mode

    ;; M-\ lsp-ui-doc-glance

    (defun jh-coding/pre-init-lsp-mode ()
      (spacemacs|use-package-add-hook lsp-mode
        :post-init
        (setq lsp-keymap-prefix "M-c l"
              lsp-headerline-breadcrumb-enable t ; Breadcrumb trail
              lsp-headerline-breadcrumb-icons-enable nil
              ;; lsp-headerline-breadcrumb-segments '(symbols) ; namespace & symbols, no file path

              ;; lsp-lens-enable nil ; default t
              ;; lsp-semantic-tokens-enable t ; enhance syntax highlight

              ;; lsp-idle-delay 0.2  ; smooth LSP features response
              lsp-eldoc-enable-hover nil ; disable all hover actions
              ;; lsp-modeline-code-actions-segments '(count icon)
              ;; lsp-navigation 'both ; default 'both ; 'simple or 'peek

              lsp-modeline-diagnostics-enable nil
              lsp-modeline-code-actions-enable nil
              )

        (setq lsp-completion-provider :none) ;; we use Corfu!

        ;; https://github.com/minad/corfu/wiki#basic-example-configuration-with-orderless
        ;; (defun my/lsp-mode-setup-completion ()
        ;;   (setf (alist-get 'styles (alist-get 'lsp-capf completion-category-defaults))
        ;;         '(flex))) ;; Configure flex
        (defun my/lsp-mode-setup-completion ()
          (setf (alist-get 'styles (alist-get 'lsp-capf completion-category-defaults))
                '(orderless))) ;; Configure orderless

        (add-hook 'lsp-mode-hook #'lsp-completion-mode) ;; important
        (add-hook 'lsp-completion-mode-hook #'my/lsp-mode-setup-completion)
        )
      )

    (defun jh-coding/post-init-lsp-mode ()
      ;; Without it lsp flycheck is not working
      (require 'lsp-headerline)
      (require 'lsp-diagnostics)

      ;; Only enable this for debugging. It makes LSP buffers really slow.
      ;; (setq lsp-log-io t)

      ;; (require 'dash)
      ;; (require 'goto-addr)
      (define-key lsp-mode-map (kbd "C-x l") lsp-command-map)
      ;; (add-hook 'xref-after-return-hook 'recenter)
      (defun kimim/lsp-find-definition-other-window ()
        (interactive)
        (lsp-find-definition :display-action 'window)
        (other-window 1))
      (define-key lsp-mode-map (kbd "C-x .") 'kimim/lsp-find-definition-other-window)
      )

    ;;;;; lsp-ui

    (defun jh-coding/post-init-lsp-ui ()
      (setq lsp-ui-doc-enable t  ; default t - doc hover popups
            lsp-ui-peek-enable t ; popups for refs, errors, symbols, etc.
            lsp-ui-peek-always-show nil ; default nil
            lsp-ui-doc-show-with-mouse t ; default t, but ugly face
            ;; lsp-ui-doc-show-with-cursor t ; default nil

            lsp-ui-doc-alignment 'window ; default 'frame
            lsp-ui-doc-text-scale-level -1 ; default 0
            lsp-ui-doc-position 'at-point ; default top
            lsp-ui-doc-max-width 120 ; default 150
            lsp-ui-doc-use-webkit nil ; default nil
            lsp-ui-doc-delay 0.2 ; default 0.2

            lsp-ui-doc-header t  ; defailt nil
            lsp-ui-doc-include-signature t ; default nil

            ;; lsp-ui-peek-list-width 40 ; default 50
            ;; lsp-ui-peek-show-directory nil ; default t
            ;; Expand all peek folds.
            ;; lsp-ui-peek-expand-function (lambda (xs) (mapcar #'car xs))
            )

      (setq lsp-ui-sideline-diagnostic-max-line-length 80 ; default 100
            lsp-ui-sideline-delay 1.0 ; default 0.2
            ;; lsp-ui-sideline-show-diagnostics t ; default t
            ;; lsp-ui-sideline-show-code-actions t ; default nil
            ;; lsp-ui-sideline-show-hover nil ; default nil
            )

      (unless (display-graphic-p) ; terminal
        ;; lsp-ui-sideline works well, but it has conflict with 'corfu terminal'
        (setq lsp-ui-sideline-enable nil) ;; sidebar code actions visual indicator
        )

      ;; (defun hidden-lsp-ui-sideline ()
      ;;   (interactive)
      ;;   (if (< (window-width) 180)
      ;;       (progn
      ;;         (setq lsp-ui-sideline-show-code-actions nil)
      ;;         (setq lsp-ui-sideline-show-diagnostics nil)
      ;;         (setq lsp-ui-sideline-show-hover nil)
      ;;         (setq lsp-ui-sideline-show-symbol nil))
      ;;     (progn
      ;;       (setq lsp-ui-sideline-show-code-actions nil)
      ;;       ;; (setq lsp-ui-sideline-show-diagnostics t)
      ;;       (setq lsp-ui-sideline-show-hover t)
      ;;       ;; (setq lsp-ui-sideline-show-symbol t)
      ;;       )))

      ;; (when (display-graphic-p) ; gui
      ;;   (advice-add 'lsp-ui-sideline--run :after 'hidden-lsp-ui-sideline)
      ;;   )
      )

    ;;;;; dap-mode

    (defun jh-coding/post-init-dap-mode ()
      (dap-mode -1)
      (dap-ui-mode -1)
      )

    ;;;;; lsp-treemacs

    (defun jh-coding/post-init-lsp-treemacs ()
      ;; limit errors to current project
      (setq lsp-treemacs-error-list-current-project-only t)
      (lsp-treemacs-sync-mode 1)
      )

    ;;;;;  DONT eglot

    ;; (defun jh-coding/init-eglot ()
    ;;   (use-package eglot
    ;;    :init
    ;;    (progn
    ;;      (setq eglot-autoshutdown t) ;; shutdown after closing the last managed buffer
    ;;      ;; (setq eglot-sync-connect 0) ;; async, do not block
    ;;      (setq eglot-extend-to-xref t) ;; can be interesting!
    ;;      (setq eglot-send-changes-idle-time 0.7)
    ;;      )
    ;;    :hook (eglot-managed-mode . jf/eglot-eldoc)
    ;;    ;; (eglot-managed-mode . jf/eglot-capf))
    ;;    :preface
    ;;    (defun jf/eglot-eldoc ()
    ;;      ;; https://www.masteringemacs.org/article/seamlessly-merge-multiple-documentation-sources-eldoc
    ;;      (setq eldoc-documentation-strategy
    ;;          'eldoc-documentation-compose))
    ;;    :bind (:map eglot-mode-map
    ;;            ("C-c ." . eglot-code-actions)
    ;;            ("C-c e r" . eglot-rename)
    ;;            ("C-c e f" . eglot-format)
    ;;            ("C-c e d"   . eglot-find-typeDefinition)
    ;;            ("C-c e E"   . eldoc)
    ;;            ("M-?" . mu-project-find-regexp)
    ;;            ;;("M-." . xref-find-definitions)
    ;;            ;;("C-c x a" . xref-find-apropos)
    ;;            ("C-c f n" . flymake-goto-next-error)
    ;;            ("C-c f p" . flymake-goto-prev-error)
    ;;            ("C-c f d" . flymake-show-project-diagnostics))
    ;;    :config

    ;;    ;; https://github.com/minad/corfu/wiki
    ;;    ;; Option 1: Specify explicitly to use Orderless for Eglot
    ;;    (add-to-list 'completion-category-overrides '((eglot (styles orderless))))

    ;;    ;; Option 2: Undo the Eglot modification of completion-category-defaults

    ;;    ;; Enable cache busting, depending on if your server returns
    ;;    ;; sufficiently many candidates in the first place.
    ;;    ;; TODO need to test!
    ;;   (advice-add 'eglot-completion-at-point :around #'cape-wrap-buster)

    ;;    ;; eglot for deno
    ;;    (defclass eglot-deno (eglot-lsp-server) () :documentation "A custom class for deno lsp.")
    ;;    (cl-defmethod eglot-initialization-options ((server eglot-deno))
    ;;      "Passes through required deno initialization options"
    ;;      (list :enable t :lint t))

    ;;    ;; config nil
    ;;    ;; importMap nil
    ;;    ;; unstable nil
    ;;    ;; codeLens t

    ;;    (add-to-list 'eglot-server-programs
    ;;             '((
    ;;               js-mode js2-mode js3-mode js-ts-mode
    ;;               typescript-mode typescript-ts-mode
    ;;               tsx-ts-mode
    ;;               ) . (eglot-deno "deno" "lsp")))

    ;;    ;; (defun deno-project-p ()
    ;;    ;;   "Predicate for determining if the open project is a Deno one."
    ;;    ;;   (let ((p-root (cdr (project-current))))
    ;;    ;;     (file-exists-p (concat p-root "deno.json"))))

    ;;    ;; (defun node-project-p ()
    ;;    ;;   "Predicate for determining if the open project is a Node one."
    ;;    ;;   (let ((p-root (cdr (project-current))))
    ;;    ;;     (file-exists-p (concat p-root "package.json"))))

    ;;    ;; (defun es-server-program (_)
    ;;    ;;   "Decide which server to use forECMAScript based on project characteristics."
    ;;    ;;   (cond ((deno-project-p) '("deno" "lsp" :initializationOptions (:enable t :lint t)))
    ;;    ;;         ((node-project-p) '("typescript-language-server" "--stdio"))
    ;;    ;;         (t                nil)))
    ;;    ;; (add-to-list 'eglot-server-programs '((js-mode js2-mode js3-mode js-ts-mode
    ;;    ;;                                                typescript-ts-mode
    ;;    ;;                                                tsx-ts-mode
    ;;    ;;                                                typescript-mode) . es-server-program))

    ;;    ;; https://zenn.dev/hyakt/articles/5c947cc22c4bfa
    ;;    (defun advice-eglot--xref-make-match (old-fn name uri range)
    ;;      (cond
    ;;       ((string-prefix-p "deno:/" uri)
    ;;        (let ((contents (jsonrpc-request (eglot--current-server-or-lose)
    ;;                              :deno/virtualTextDocument
    ;;                              (list :textDocument (list :uri uri))))
    ;;            (filepath (concat (temporary-file-directory)
    ;;                        (replace-regexp-in-string "^deno:/\\(.*\\)$" "\\1" (url-unhex-string uri)))))
    ;;          (unless (file-exists-p filepath)
    ;;            (make-empty-file filepath 't)
    ;;            (write-region contents nil filepath nil 'silent nil nil))
    ;;          (apply old-fn (list name filepath range))))
    ;;       (t
    ;;        (apply old-fn (list name uri range)))))
    ;;    (advice-add 'eglot--xref-make-match :around #'advice-eglot--xref-make-match)

    ;;    ;; source: https://manueluberti.eu/2022/09/01/consult-xref.html
    ;;    (defun mu-project-find-regexp ()
    ;;      "Use `project-find-regexp' with completion."
    ;;      (interactive)
    ;;      (defvar xref-show-xrefs-function)
    ;;      (let ((xref-show-xrefs-function #'consult-xref))
    ;;        (if-let ((tap (thing-at-point 'symbol)))
    ;;           (project-find-regexp tap)
    ;;          (call-interactively #'project-find-regexp))))


    ;;  (add-to-list 'eglot-server-programs
    ;;           `(ruby-mode . ("solargraph" "socket" "--port" :autoport)))
    ;;  (add-to-list 'eglot-server-programs
    ;;           `(ruby-ts-mode . ("solargraph" "socket" "--port" :autoport)))

    ;;  ;; 서버 프로그램을 명시해야 할 경우
    ;;  ;; (add-to-list 'eglot-server-programs
    ;;  ;;              '(yaml-mode . ("yaml-language-server" "--stdio")))

    ;;  ;; No event buffers, disable providers cause a lot of hover traffic. Shutdown unused servers.
    ;;  ;; (setq eglot-events-buffer-size 0
    ;;  ;;       eglot-ignored-server-capabilities '(:hoverProvider
    ;;  ;;                                           :documentHighlightProvider))
    ;;  )
    ;; )

    ;;;;; Formmter
    ;;;;;;  apheleia

    ;; /meain-dotfiles/emacs/.config/emacs/init.el

    (defun jh-coding/init-apheleia ()
      (use-package apheleia
        :after evil
        :commands (apheleia-format-buffer my/format-buffer)
        :config
        (add-hook 'markdown-mode-hook 'apheleia-mode)
        (add-hook 'yaml-mode-hook 'apheleia-mode)

        ;; js2-mode 는 따로 넣어줘야 한다.
        (add-hook 'js2-mode-hook 'apheleia-mode)
        (setf  (alist-get 'js2-mode apheleia-mode-alist) '(prettier-javascript))
        )
      )

    ;;    :config
    ;;    (setf  (alist-get 'json-mode apheleia-mode-alist) '(prettier))
    ;;    (setf  (alist-get 'yaml-mode apheleia-mode-alist) '(prettier))
    ;; (setf  (alist-get 'html-mode apheleia-mode-alist) '(prettier))
    ;;    (setf  (alist-get 'typescript-mode apheleia-mode-alist) '(prettier))
    ;;    (setf  (alist-get 'js-mode apheleia-mode-alist) '(prettier))
    ;;    (setf  (alist-get 'web-mode apheleia-mode-alist) '(prettier))
    ;;    (setf  (alist-get 'markdown-mode apheleia-mode-alist) '(prettier))

    ;; 안쓴다. LSP 로 한다.
    ;;    ;; clojure
    ;;    ;; (setf (alist-get 'zprint apheleia-formatters)
    ;;    ;;       '("zprint"))
    ;;    ;; (setf (alist-get 'clojure-mode apheleia-mode-alist)
    ;;    ;;       '(zprint))

    ;;     (setf (alist-get 'shell-script-mode apheleia-mode-alist)
    ;;           '(shfmt))
    ;;     (setf (alist-get 'sh-mode apheleia-mode-alist)
    ;;           '(shfmt))
    ;;     ;; (setf (alist-get 'isort apheleia-formatter)
    ;;     ;;       '("isort" "--stdout" "-"))
    ;;     ;; (setf (alist-get 'python-mode apheleia-mode-alist)
    ;;     ;;       '(isort black))
    ;;     )
    ;;   )

    ;;;;;; DONT format-all

    ;; install tidy-html5

    ;; format-all-mode 로 등록을 하면 auto-format on save 가 된다.
    ;; (defun jh-coding/init-format-all ()
    ;;   (use-package format-all
    ;;     :ensure t
    ;;     :hook ((markdown-mode . format-all-mode) ; auto-format on save
    ;;            (yaml-mode . format-all-mode)
    ;;            (json-mode . format-all-mode)
    ;;            ;; (emacs-lisp-mode . format-all-mode)
    ;;            (format-all-mode . format-all-ensure-formatter))
    ;;:init
    ;; (defun my/format-all-code ()
    ;;   "Auto-format whole buffer."
    ;;   (interactive)
    ;;   (if (derived-mode-p 'prolog-mode)
    ;;       (prolog-indent-buffer)
    ;;     (format-all-buffer)))
    ;;     )
    ;;   )

    ;;;;; DONT asdf (not linux)

    ;; (defun jh-coding/init-asdf ()
    ;;   (use-package asdf
    ;;     :demand t
    ;;     :if (not (or *is-windows* my/remote-server *is-termux*))
    ;;     :config (asdf-enable)))

    ;;;;; direnv

    (defun jh-coding/init-direnv ()
      (use-package direnv
        :demand t
        :if (not (or *is-windows* my/remote-server *is-termux*))
        :config
        (direnv-mode)))

    ;;;;; DONT jsonian

    ;; (defun jh-coding/init-jsonian ()
    ;;   (use-package jsonian
    ;;     :after so-long
    ;;     :defer 10
    ;;     :custom (jsonian-no-so-long-mode)
    ;;     :config
    ;;     (defun json-pretty (start end)
    ;;       (interactive "*r")
    ;;       (replace-string "\\\"" "\"" nil start end)
    ;;       (json-pretty-print-buffer)
    ;;       )

    ;;     (defun temp-json-pretty-buffer ()
    ;;       "json pretty print buffer"
    ;;       (interactive)
    ;;       (temp-buffer)
    ;;       (require 'jsonian-mode)
    ;;       (jsonian-mode)
    ;;       (yank-pop)
    ;;       (json-pretty (region-beginning) (region-end))
    ;;       )
    ;;     )
    ;;   )

    ;;;;; DONT prodigy

    ;; (defun jh-coding/post-init-prodigy ()
    ;;   (progn
    ;;     ;; define service
    ;;     ;; -D, --buildDrafts                include content marked as draft
    ;;     ;; -E, --buildExpired               include expired content
    ;;     ;; -F, --buildFuture                include content with publishdate in the future

    ;;     (prodigy-define-service
    ;;       :name "Hugo Server"
    ;;       :command "hugo"
    ;;       ;; :args '("server" "-D")
    ;;       :args '("server" "-D")
    ;;       :cwd blog-admin-dir
    ;;       :tags '(hugo server)
    ;;       :kill-signal 'sigkill
    ;;       :kill-process-buffer-on-stop t)
    ;;     ))

    ;;;; Shell

    ;;;;; bats-mode

    (defun jh-coding/init-bats-mode ()
      (use-package bats-mode :defer 3))

    ;;;; LISP

    ;;;;; Emacs Lisp

    (defun jh-coding/init-eros ()
      (use-package eros
        :config (eros-mode 1)))

    ;;;;; Clojure

    (defun jh-coding/init-clojars ()
      (use-package clojars))

    (defun jh-coding/init-clojure-essential-ref-nov ()
      (use-package clojure-essential-ref-nov
        :bind (:map cider-mode-map
                    ("C-h F" . clojure-essential-ref)
                    :map cider-repl-mode-map
                    ("C-h F" . clojure-essential-ref))
        :init
        (setq clojure-essential-ref-default-browse-fn #'clojure-essential-ref-nov-browse)
        (setq clojure-essential-ref-nov-epub-path (concat user-org-directory "epub/Clojure_The_Essential_Reference_v31.epub")))
      )

    (defun jh-coding/post-init-clojure-mode ()
      ;; copy from corgi
      (add-to-list 'auto-mode-alist '("\\.endl$" . clojure-mode))
      (add-to-list 'magic-mode-alist '("^#![^\n]*/\\(clj\\|clojure\\|bb\\|lumo\\)" . clojure-mode))

      ;; Because of CIDER's insistence to send forms to all linked REPLs, we
      ;; *have* to be able to switch cljc buffer to clj/cljs mode without
      ;; cider complaining.
      ;; (setq clojure-verify-major-mode nil) ; 나중에 해보고

      (setq clojure-indent-style 'align-arguments)

      ;; Vertically align s-expressions
      ;; https://github.com/clojure-emacs/clojure-mode#vertical-alignment
      (setq clojure-align-forms-automatically t)

      ;; 2023-09-15 demete evil-cleverparens and evil-lisp-state
      ;; for clojure layer only (comment out line above)
      ;; (add-hook 'spacemacs-post-user-config-hook #'spacemacs/toggle-evil-safe-lisp-structural-editing-on-register-hook-clojure-mode)

      ;; manually use on lsp mode
      ;; (remove-hook 'clojure-mode-hook 'spacemacs//clojure-setup-backend)
      )

    ;; (use-package clj-deps-new)
    ;;   ;; Emacs interface to deps-new (like lein new for Leiningen, or clj-new for Boot)
    ;;   ;; ref: https://github.com/jpe90/emacs-clj-deps-new

    (defun jh-coding/post-init-cider ()
      ;; NOTE 2022-11-21: for the linter (clj-kondo), refer to the Flymake
      ;; NOTE 2022-11-23: This is not final.  I will iterate on it over
      ;; time as I become more familiar with the requirements.
      (setq cider-repl-result-prefix ";; => "
            cider-eval-result-prefix ""
            cider-connection-message-fn nil ; cute, but no!
            cider-repl-prompt-function #'my/cider-repl-prompt
            ;; cider-use-overlays nil ; echo area is fine
            )

      ;; NOTE: formatting with LSP.
      ;; "F" #'cider-format-buffer

      (setq
       cider-prompt-for-symbol nil
       cider-repl-display-help-banner nil        ;; enable help banner
       ;; cider-print-fn 'puget                   ;; pretty printing with sorted keys / set values
       clojure-align-forms-automatically t
       ;; clojure-toplevel-inside-comment-form t
       ;; cider-result-overlay-position 'at-point   ; results shown right after expression
       ;; cider-overlays-use-font-lock t
       cider-repl-buffer-size-limit 100          ; limit lines shown in REPL buffer
       nrepl-use-ssh-fallback-for-remote-hosts t ; connect via ssh to remote hosts
       cider-preferred-build-tool 'clojure-cli
       )

      ;; Note: Ensure CIDER and lsp-mode play well together, as we use both.
      ;; - LSP for more static-analysis-y services (completions, lookups, errors etc.),
      ;; - CIDER for "live" runtime services (enhanced REPL, interactive debugger etc.).

      ;; /home/junghan/sync/man/dotsamples/vanilla/evalapply-dotfiles-clojure/init.el
      ;; Use clojure-lsp for eldoc and completions
      ;; layer 에서 eldoc 을 빼주면 된다. lsp 로만 동작하게 된다.
      ;; h/t cider docs and ericdallo/dotfiles/.config/doom/config.el
      ;; (remove-hook 'eldoc-documentation-functions #'cider-eldoc)
      ;; (remove-hook 'completion-at-point-functions #'cider-complete-at-point)

      ;; settings h/t suvratapte/dot-emacs-dot-d
      ;; (setq
      ;;  ;; cider-prompt-for-symbol nil
      ;;  ;; play nice with lsp-mode
      ;;  ;; h/t ericdallo/dotfiles/.config/doom/config.el
      ;;  cider-font-lock-dynamically nil ; use lsp semantic tokens
      ;;  cider-eldoc-display-for-symbol-at-point nil ; use lsp
      ;;  cider-prompt-for-symbol nil ; use lsp
      ;;  cider-use-xref nil ; use lsp
      ;;  ;; Maybe customize variables for cider-jack-in
      ;;  ;; https://docs.cider.mx/cider/basics/up_and_running.html
      ;;  )

      (if (package-installed-p 'corfu)
          (evil-define-key 'insert cider-repl-mode-map
            (kbd "C-j") 'corfu-next
            (kbd "C-k") 'corfu-previous))
      )
    ;;;;; DONT clojure-ts-mode

    ;; https://github.com/clojure-emacs/clojure-ts-mode
    ;; (defun jh-coding/init-clojure-ts-mode ()
    ;;   (use-package clojure-ts-mode
    ;;     :ensure t
    ;;     :init
    ;;     (setq clojure-ts-comment-macro-font-lock-body t) ; default nil
    ;;     (setq clojure-ts-indent-style 'fixed) ; default 'semantic
    ;;     ;; clojure-ts-ensure-grammars is set to t (the default).
    ;;     ;; Do not indent single ; comment characters
    ;;     ;; (add-hook 'clojure-ts-mode-hook 'cider-mode)
    ;;     ;; (add-hook 'clojure-ts-mode-hook (lambda () (setq-local comment-column 0)))
    ;;     )
    ;;   )

    ;;;; Elixir

    ;; (defun jh-coding/init-elixir-ts-mode ()
    ;;   (use-package elixir-ts-mode
    ;;     :hook
    ;;     (elixir-ts-mode
    ;;      .
    ;;      (lambda ()
    ;;        (message "=> Turn on elixir-ts-mode")
    ;;        (push '(">=" . ?\u2265) prettify-symbols-alist)
    ;;        (push '("<=" . ?\u2264) prettify-symbols-alist)
    ;;        (push '("!=" . ?\u2260) prettify-symbols-alist)
    ;;        (push '("==" . ?\u2A75) prettify-symbols-alist)
    ;;        (push '("=~" . ?\u2245) prettify-symbols-alist)
    ;;        (push '("<-" . ?\u2190) prettify-symbols-alist)
    ;;        (push '("->" . ?\u2192) prettify-symbols-alist)
    ;;        (push '("<-" . ?\u2190) prettify-symbols-alist)
    ;;        (push '("|>" . ?\u25B7) prettify-symbols-alist)))
    ;;     (elixir-ts-mode . eglot-ensure)
    ;;     (before-save . eglot-format) ; formatting
    ;;     )
    ;;   )

    ;; (defun jh-coding/init-exunit ()
    ;;   (use-package exunit)
    ;;   )

    ;;;; Documentation

    ;;;;; Consult-dash

    (defun jh-coding/init-consult-dash ()
      (use-package consult-dash
        :defer 3
        :commands (consult-dash)
        :bind ("M-s d" . consult-dash)
        :init
        (spacemacs/declare-prefix "arz" "zeal/dash docs")
        (spacemacs/set-leader-keys
          "arzH" 'consult-dash)
        :config
        (require 'dash-docs)
        ;; (setq dash-docs-docsets-path "~/.docsets/"
        ;;       dash-docs-docset-newpath "~/.docsets/")
        (setq dash-docs-browser-func 'eww)
        (setq dash-docs-enable-debugging nil)
        )
      )

    ;;;;; Devdocs-Browser

    ;; 한글 번역 문서 지원
    ;; devdocs-browser-install-doc - index 파일만 저장
    ;; devdocs-browser-download-offline-data - 오프라인 전체 데이터 저장 (기본 영어)
    ;; (setq devdocs-browser-cache-directory "~/spacemacs/.cache/private/")
    (defun jh-coding/init-devdocs-browser ()
      (use-package devdocs-browser
        :defer 2
        :bind (("M-s-," . devdocs-browser-open) ;; M-s-/ yas-next-field
               ("M-s-." . devdocs-browser-open-in))
        :config
        (add-to-list 'devdocs-browser-major-mode-docs-alist '(js2-mode "javascript" "node"))
        ;; (add-to-list 'devdocs-browser-major-mode-docs-alist '(rjsx-mode "react" "javascript" "node"))
        ;; (add-to-list 'devdocs-browser-major-mode-docs-alist '(typescript-ts-mode "typescript"))
        ;; (add-to-list 'devdocs-browser-major-mode-docs-alist '(js-ts-mode "javascript" "node"))
        )
      )


    ;;;; Tree-sitter
    ;;;;; evil-textobj-tree-sitter

    ;; evil-textobj-tree-sitter--can-use-builtin-treesit
    (defun jh-coding/init-evil-textobj-tree-sitter ()
      (use-package evil-textobj-tree-sitter
        :after evil treesit
        :config
        (require 'treesit)

        ;; bind `function.outer`(entire function block) to `f` for use in things like `vaf`, `yaf`
        (define-key evil-outer-text-objects-map "f" (evil-textobj-tree-sitter-get-textobj "function.outer"))
        ;; bind `function.inner`(function block without name and args) to `f` for use in things like `vif`, `yif`
        (define-key evil-inner-text-objects-map "f" (evil-textobj-tree-sitter-get-textobj "function.inner"))

        ;; You can also bind multiple items and we will match the first one we can find
        (define-key evil-outer-text-objects-map "a" (evil-textobj-tree-sitter-get-textobj ("conditional.outer" "loop.outer")))
        )
      )

    ;;;;; TODO ts-fold with treesit.el

    ;; (with-eval-after-load 'hydra
    ;;   (defhydra hydra-ts-fold (:exit t :hint nil)
    ;;     "
    ;; Tree-sitter code folding
    ;; Point^^                     Recursive^^             All^^
    ;; ^^^^^^---------------------------------------------------------------
    ;; [_f_] toggle fold at point
    ;; [_o_] open at point         [_O_] open recursively  [_M-o_] open all
    ;; [_c_] close at point         ^ ^                    [_M-c_] close all"
    ;;     ("f" ts-fold-toggle)
    ;;     ("o" ts-fold-open)
    ;;     ("c" ts-fold-close)
    ;;     ("O" ts-fold-open-recursively)
    ;;     ("M-o" ts-fold-open-all)
    ;;     ("M-c" ts-fold-close-all)))

    ;; (defun jh-coding/init-ts-fold ()
    ;;   (use-package ts-fold
    ;;     :after treesit
    ;;     :config (global-ts-fold-indicators-mode)
    ;;     :bind (("M-g 0" . hydra-ts-fold/body))
    ;;     ))

    ;; <treesit-fold>

    (with-eval-after-load 'hydra
      (defhydra hydra-treesit-fold (:exit t :hint nil)
        "
    Treesit-fold
    Point^^                     Recursive^^             All^^
    ^^^^^^---------------------------------------------------------------
    [_f_] toggle fold at point
    [_o_] open at point         [_O_] open recursively  [_M-o_] open all
    [_c_] close at point         ^ ^                    [_M-c_] close all"
        ("f" treesit-fold-toggle)
        ("o" treesit-fold-open)
        ("c" treesit-fold-close)
        ("O" treesit-fold-open-recursively)
        ("M-o" treesit-fold-open-all)
        ("M-c" treesit-fold-close-all)))

    (defun jh-coding/init-treesit-fold ()
      (use-package treesit-fold
        :after treesit
        :config (global-treesit-fold-indicators-mode)
        :bind (("M-g 0" . hydra-treesit-fold/body))
        ))

    ;;;;; DONT combobulate

    ;; (defun jh-coding/init-combobulate ()
    ;;   (use-package combobulate
    ;;     :if (not (or my/remote-server my/is-termux))
    ;;     :commands combobulate-mode
    ;;     :hook ((python-ts-mode . combobulate-mode)
    ;;            (js-ts-mode . combobulate-mode)
    ;;            (css-ts-mode . combobulate-mode)
    ;;            (yaml-ts-mode . combobulate-mode)
    ;;            (typescript-ts-mode . combobulate-mode)
    ;;            (tsx-ts-mode . combobulate-mode)
    ;;            )
    ;;     :custom
    ;;     (combobulate-key-prefix "C-c t")
    ;;     )
    ;;   )

    ;;;; Misc
    ;;;;; Exercism

    (defun jh-coding/init-exercism ()
      (use-package exercism
        ;; :if (not (or *is-windows* my/remote-server *is-termux*))
        :defer 3
        :custom (exercism-display-tests-after-run t))
      )

    ;;;;; Leetcode

    ;; junghanacs@gmail.com gpg
    ;; pipx install my_cookies --force
    (defun jh-coding/init-leetcode ()
      (use-package leetcode
        :defer 3
        :init
        (setq leetcode-prefer-language "javascript")
        (setq leetcode-prefer-sql "mysql")
        (setq leetcode-save-solutions t)
        (setq leetcode-directory "~/leetcode")
        (setq leetcode-show-problem-by-slug t)
        :config
        (add-hook 'leetcode-solution-mode-hook
                  (lambda() (flycheck-mode -1)))
        ))

    ;;;; ts-mode

    ;; awk-yasnippets
    (defun jh-coding/init-awk-ts-mode ()
      (use-package awk-ts-mode))

    ```


#### <span class="section-num">3.9.3</span> The `jh-coding` funcs.el {#h:5d81e303-afbd-493a-b526-19cbb5a34417}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; Utility

;;;###autoload
(defun my/format-buffer ()
  "Format a buffer."
  (interactive)
  (cond
   ((eq major-mode 'emacs-lisp-mode)
    (indent-region (point-min) (point-max)))
   ((eq major-mode 'ledger-mode)
    (ledger-mode-clean-buffer))
   (t (call-interactively 'apheleia-format-buffer))))

;;; Clojure

(defun clojars-find ()
  "Lookup for symbol at point on clojars. Useful for updating packages in project.clj"
  (interactive)
  (clojars (symbol-at-point)))

(defun my/cider-repl-prompt (namespace)
  "Return a prompt string that mentions NAMESPACE."
  (format "%s🦄 " (cider-abbreviate-ns namespace)))

(defun mpereira/cider-eval-sexp-at-point (&optional output-to-current-buffer)
  "Evaluate the expression around point.
If invoked with OUTPUT-TO-CURRENT-BUFFER, output the result to current buffer."
  (interactive "P")
  (save-excursion
    (goto-char (- (cadr (cider-sexp-at-point 'bounds))
                  1))
    (cider-eval-last-sexp output-to-current-buffer)))

;; Portal Data Inspector
;; def portal to the dev namespace to allow dereferencing via @dev/portal
(defun portal.api/open ()
  (interactive)
  (cider-nrepl-sync-request:eval
   "(do (ns dev) (def portal ((requiring-resolve 'portal.api/open))) (add-tap (requiring-resolve 'portal.api/submit)))"))

(defun portal.api/clear ()
  (interactive)
  (cider-nrepl-sync-request:eval "(portal.api/clear)"))

(defun portal.api/close ()
  (interactive)
  (cider-nrepl-sync-request:eval "(portal.api/close)"))

;; ---------------------------------------
;; Custom functions

;; toggle reader macro sexp comment
;; toggles the #_ characters at the start of an expression
(defun clojure-toggle-reader-comment-sexp ()
  (interactive)
  (let* ((point-pos1 (point)))
    (evil-insert-line 0)
    (let* ((point-pos2 (point))
           (cmtstr "#_")
           (cmtstr-len (length cmtstr))
           (line-start (buffer-substring-no-properties point-pos2 (+ point-pos2 cmtstr-len)))
           (point-movement (if (string= cmtstr line-start) -2 2))
           (ending-point-pos (+ point-pos1 point-movement 1)))
      (if (string= cmtstr line-start)
          (delete-char cmtstr-len)
        (insert cmtstr))
      (goto-char ending-point-pos)))
  (evil-normal-state))
;;
;; Toggle view of a clojure `(comment ,,,) block'
(defun clojure-hack/toggle-comment-block (arg)
  "Close all top level (comment) forms. With universal arg, open all."
  (interactive "P")
  (save-excursion
    (goto-char (point-min))
    (while (search-forward-regexp "^(comment\\>" nil 'noerror)
      (call-interactively
       (if arg 'evil-open-fold
         'evil-close-fold)))))

;;; TODO lsp-imenu

;; /vanilla/mpereira-dotfiles-evil-clojure/configuration.org
;; (defun mpereira/lsp-imenu ()
;;   (interactive)
;;   (lsp-request-async
;;    "textDocument/documentSymbol"
;;    (lsp-make-document-symbol-params
;;     :text-document
;;     (lsp--text-document-identifier))
;;    (lambda (document-symbols)
;;      (let ((symbols
;;             (mapcar
;;              (lambda (symbol)
;;                (let* ((name (gethash "name" symbol))
;;                       (kind (gethash "kind" symbol))
;;                       (position (or (-some->> symbol
;;                                       lsp:document-symbol-selection-range
;;                                       lsp:range-start)
;;                                     (-some->> symbol
;;                                       lsp:symbol-information-location
;;                                       lsp:location-range
;;                                       lsp:range-start)
;;                                     (error "Unable to go to location")))
;;                       (point (lsp--position-to-point position))
;;                       (kind-name (inflection-singularize-string
;;                                   (lsp--imenu-kind->name kind))))
;;                  (cons (concat (propertize
;;                                 kind-name
;;                                 'face
;;                                 (cond
;;                                  ((string= kind-name "Function")
;;                                   'font-lock-function-name-face)
;;                                  ((string= kind-name "Constant")
;;                                   'font-lock-constant-face)
;;                                  (t
;;                                   'font-lock-keyword-face)))
;;                                " "
;;                                name)
;;                        (list kind point))))
;;              document-symbols)))
;;        (let* ((symbol-name (consult--read
;;                             (sort symbols
;;                                   (lambda (a b)
;;                                     (< (nth 1 (cdr a))
;;                                        (nth 1 (cdr b)))))
;;                             :prompt "Go to symbol: "
;;                             :require-match t
;;                             :sort nil
;;                             :category 'foo))
;;               (symbol-data (alist-get symbol-name symbols nil nil 'equal))
;;               (kind (nth 0 symbol-data))
;;               (position (nth 1 symbol-data)))
;;          (goto-char position))))
;;    :mode 'alive))

```


#### <span class="section-num">3.9.4</span> The `jh-coding` config.el {#h:0e00f017-7836-4f4a-8a46-2250b0368cd4}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; Do not indent single ; comment characters
;; TODO reframe imenu : check agzam's dot

;;; Clojure Hook
(dolist (hook '(clojure-mode-hook clojurescript-mode-hook))
  (add-hook hook
            (lambda ()
              (setq-local comment-column 0)
              (setq-local zeal-at-point-docset '("clojure"))
              (setq-local consult-dash-docsets '("Clojure" "ClojureEssentialReference")))))
(add-hook 'clojurescript-mode-hook #'cider-mode)

;;; consult-dash
(add-hook 'emacs-lisp-mode-hook (lambda ()
                                  (setq-local consult-dash-docsets '("Emacs_Lisp"))
                                  (define-key emacs-lisp-mode-map (kbd "M-[") 'backward-sexp)
                                  (define-key emacs-lisp-mode-map (kbd "M-]") 'forward-sexp)))

(add-hook 'python-mode-hook (lambda () (setq-local consult-dash-docsets '("Python_3" "NumPy"))))

(add-hook 'racket-mode-hook (lambda () (setq-local consult-dash-docsets '("Racket"))))

(add-hook 'elixir-ts-mode-hook (lambda ()
                                 (setq-local zeal-at-point-docset '("elixir"))
                                 (setq-local consult-dash-docsets '("Elixir"))))


(add-hook 'js2-mode-hook (lambda () (setq-local consult-dash-docsets '("JavaScript" "NodeJS"))))

;; (add-hook 'shell-mode-hook (lambda ()
;;                               (setq-local consult-dash-docsets '("Bash"))))

;;; flycheck

;; (with-eval-after-load 'flycheck
;;   (add-to-list 'flycheck-disabled-checkers 'javascript-jshint)
;;   (add-to-list 'flycheck-disabled-checkers 'javascript-jscs)
;;   (flycheck-add-mode 'javascript-eslint 'web-mode)
;;   )

;;; org-babel

(setq org-babel-js-function-wrapper
      "process.stdout.write(require('util').inspect(function(){\n%s\n}(), { maxArrayLength: null, maxStringLength: null, breakLength: Infinity, compact: true }))")
```


#### <span class="section-num">3.9.5</span> The `jh-coding` keybindings.el {#h:e07355aa-b43f-4b0f-8ca5-98dba9ec3e74}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;;; Formater

;; (global-set-key (kbd "M-g =") 'format-all-region-or-buffer) ; format-all-region
;; (global-set-key (kbd "M-g M-=") #'my/format-all-code)

(global-set-key (kbd "M-g =") 'my/format-buffer)

;;; LSP

(with-eval-after-load 'lsp-ui
  (evil-define-key '(insert normal) lsp-mode-map (kbd "M-\\") 'lsp-ui-doc-glance)
  (evil-define-key '(normal visual) lsp-ui-doc-frame-mode-map (kbd "M-\\") 'lsp-ui-doc-unfocus-frame)

  (evil-define-key '(insert normal) lsp-mode-map (kbd "C-'") 'lsp-ui-doc-glance)
  (evil-define-key '(normal visual) lsp-ui-doc-frame-mode-map (kbd "C-'") 'lsp-ui-doc-unfocus-frame)

  (define-key lsp-ui-peek-mode-map (kbd "C-j") 'lsp-ui-peek-select-next)
  (define-key lsp-ui-peek-mode-map (kbd "C-k") 'lsp-ui-peek-select-prev)
  )

;;; Clojure

(spacemacs/set-leader-keys "opp" 'portal.api/open)
(spacemacs/set-leader-keys "opc" 'portal.api/clear)
(spacemacs/set-leader-keys "opD" 'portal.api/close)

;; Lookup functions in Clojure - The Essentail Reference book
;; https://github.com/p3r7/clojure-essential-ref
(spacemacs/set-leader-keys "or" 'clojure-essential-ref-nov)

;; Clojure Essentail Ref lookup
(spacemacs/set-leader-keys-for-minor-mode 'clojure-mode "hr" 'clojure-essential-ref-nov)

(spacemacs/set-leader-keys-for-major-mode 'clojure-mode "C" 'clojars-find)

;; Assign keybinding to the toggle-reader-comment-sexp function
(define-key global-map (kbd "C-#") 'clojure-toggle-reader-comment-sexp)

(evil-define-key 'normal clojure-mode-map (kbd "M-\\") 'lsp-ui-doc-glance)

;; 'mpereira/cider-eval-sexp-at-point
(evil-define-key 'normal clojure-mode-map
  "zC" 'clojure-hack/toggle-comment-block
  "zO" (lambda () (interactive) (clojure-hack/toggle-comment-block 'open)))

;; 'SPC r y' consult-yank-from-kill-ring
;; (add-hook 'clojure-mode-hook (lambda ()
;;                                (define-key clojure-mode-map (kbd "M-y") 'consult-yank-pop)
;;                                (define-key clojure-mode-map (kbd "M-o") 'embark-act)
;;                                ))

;;; Documentation

(spacemacs/set-leader-keys "ar." #'devdocs-browser-open)
(spacemacs/set-leader-keys "o." #'devdocs-browser-open-in)

;;; Exercism / Leetcode

(global-set-key (kbd "M-c e") 'exercism)
(spacemacs/set-leader-keys "a E" 'exercism)

;; (spacemacs/set-leader-keys "o9" 'sort-lines)

(global-set-key (kbd "M-c l") 'leetcode)

;; Labels the app as Leetcode so it doesn't appear as "prefix" in the menu
(spacemacs/declare-prefix "a L" "Leetcode")

;; The remaining useful keybindings to using Leetcode
(spacemacs/set-leader-keys
  "a L l" 'leetcode
  "a L d" 'leetcode-show-current-problem
  "a L r" 'leetcode-refresh
  "a L t" 'leetcode-try
  "a L u" 'leetcode-submit
  )

```


### <span class="section-num">3.10</span> The `jh-org` layer {#h:46917039-0dd6-40dd-955a-2e5006233279}




#### <span class="section-num">3.10.1</span> The `jh-org` layer.el {#h:69c65439-dee8-45f6-9a08-4bff03a6cd71}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   ;; not toc-org org-superstar
   (spacemacs-org :packages (default-org-config)) ; done

   (org
    ;; evil-org org-present
    ;; evil-surround gnuplot htmlize ob org-contrib org-download org-mime
    ;; org-cliplink org-rich-yank org-epub
    ;; org-twbs org-gfm ox-hugo org-sticky-header
    ;; org-roam org-roam-ui ox-asciidoc org-pomodoro
    :packages (not helm helm-org-rifle ox-hugo org-journal persp-mode company
                   emoji-cheat-sheet-plus company-emoji)
    :variables
    org-enable-roam-support t
    org-want-todo-bindings t ; use evil-binding (t, T, M-t)
    org-enable-epub-support t
    org-enable-transclusion-support t
    org-enable-asciidoc-support t
    org-enable-org-contacts-support t
    org-enable-github-support t ; gfm markdown support

    ;; should be nil
    org-enable-roam-protocol nil
    org-enable-roam-ui nil
    org-enable-org-brain-support nil
    org-enable-hugo-support nil ; use custom version
    org-enable-modern-support nil
    org-enable-org-journal-support nil
    org-enable-sticky-header nil ; use my-breadcrumbs
    org-enable-reveal-js-support nil ; 2023-07-06 use ox-reveal instead
    org-enable-appear-support nil ; 2023-06-17 nil
    ;; ;; org-appear-trigger 'manual ; for evil editing
    org-todo-dependencies-strategy nil ; 'naive-auto
    org-enable-valign nil ; performance issue, should be nil
    org-enable-trello-support nil ; should be nil

    ;; org-directory "~/sync/org/" ; (file-truename "~/sync/org/")

    org-directory user-org-directory

    org-hugo-base-dir (file-truename "~/git/blog/")
    ;; org-hugo-base-dir blog-admin-dir

    org-roam-directory (concat org-directory "roam/")
    org-notes-directory (concat org-roam-directory "notes/")

    org-workflow-directory (concat org-roam-directory "workflow/")
    org-projectile-file (file-truename (concat org-workflow-directory "20230101T010100--project__agenda.org"))

    ;; org-persp-startup-org-file org-projectile-file
    ;; org-persp-startup-with-agenda " " ; "a"

    org-contacts-files (list (concat org-workflow-directory "20230303T030300--contacts__agenda.org"))
    org-contacts-icon-use-gravatar nil

    org-contact-file (concat org-workflow-directory "20230303T030300--contacts__agenda.org")
    org-iam-file (concat org-workflow-directory "20231213T133300--iam.org")
    org-quote-file (concat org-workflow-directory "quote.org")
    org-mobile-file (concat org-workflow-directory "mobile.org")
    org-refile-file (concat org-workflow-directory "20230202T020200--inbox__refile.org")
    org-links-file (concat org-workflow-directory "20230219T035500--links__agenda.org")
    org-tags-file (concat org-notes-directory "20231005T133900--filetags__index_terms.org")
    org-now-file (concat org-notes-directory "20230810T101300--now__project.org")

    ;; org-user-agenda-files (list (concat org-roam-directory "workflow/"))
    org-user-agenda-files (append (file-expand-wildcards (concat org-roam-directory "workflow/*.org")))

    org-log-file (concat org-workflow-directory "20220101T010100--log__agenda.org")
    ;; org-user-agenda-files (file-expand-wildcards "~/sync/org/workflow/*.org")
    org-user-agenda-diary-file org-log-file
    )
   )
 )
```


#### <span class="section-num">3.10.2</span> The `jh-org` packages.el {#h:cbdd1ab0-bbce-4589-89fb-838d54e320ec}

<!--list-separator-->

1.  Packages

    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-org-packages
      '(

    ;;;; Built-in Pkgs

        (org-attach :location built-in)
        (remember :location built-in)

    ;;;; Spacemacs Pkgs

        org
        org-roam

        org-download

        org-projectile
        org-present
        ;; org-superstar

    ;;;; Additional Packages

        ;; bbdb
        math-preview ; for org and markdown with mathjax

        citar
        citar-embark
        org-randomnote

        ;; https://github.com/tecosaur/org-glossary/issues/9
        (ox-hugo :location (recipe :fetcher github :repo
                                   "kaushalmodi/ox-hugo"
                                   :branch "fix-custom-link-exports"
                                   ))

        (org-remoteimg :location (recipe :fetcher github :repo "gaoDean/org-remoteimg"))
        (org-imgtog :location (recipe :fetcher github :repo "gaoDean/org-imgtog"))
        orglink
        org-ql

        org-gcal ; sync google-calendar

        ;; org-super-agenda
        org-reverse-datetree

        ob-translate
        ob-mermaid
        ;; ob-d2
        (ob-racket :location (recipe :fetcher github :repo "DEADB17/ob-racket"))

        org-make-toc

        ;; toc-org

        org-tidy
        ox-reveal
        org-tree-slide
        (org-remark :location (recipe :fetcher github :repo "nobiot/org-remark"))

        ;; (zotxt :location (recipe :fetcher github :repo "egh/zotxt-emacs"))
        ;; (bibtex-capf :location (recipe :fetcher github :repo "mclear-tools/bibtex-capf"))

        org-drill
        (org-fc :location (recipe :fetcher git
                                  :url "https://git.sr.ht/~l3kn/org-fc"
                                  :files (:defaults "awk" "docs" "demo.org")))
        ;; org-anki

        (org-pandoc-import :location (recipe :fetcher github :repo "tecosaur/org-pandoc-import"
                                             :files ("*.el" "*.org" "filters" "preprocessors")))

        org-rich-yank
        yankpad

        (orgabilize :location (recipe :fetcher github :repo "akirak/orgabilize.el"))
        ;; (org-web-tools :location (recipe :fetcher github :repo "alphapapa/org-web-tools"
        ;;                            :files ("*.el" "*.org")))

        wikinfo   ; wiki info-mode
        wikinforg

    ;;;; PKM

        ;; zk

        consult-notes
        side-notes
        (org-glossary :location (recipe :fetcher github :repo "tecosaur/org-glossary"
                                        :files ("*.el" "*.org" "*.texi")))

        denote
        ;; (denote :location (recipe :fetcher github :repo "protesilaos/denote"
        ;;                     :files ("*.el" "*.md" "*.org" "*.texi" "tests/*.el")))

        tmr
        citar-denote
        ;; (denote-explore :location (recipe :fetcher github :repo "pprevos/denote-explore"
        ;;                                   :files ("*.el" "*.org" "*.R" "*.png")))
        ;; denote-refs

        ;; citar-org-roam
        ;; consult-org-roam

    ;;;; HOLD Waitings

        ;; (org-clock-budget :location (recipe :fetcher github :repo "Fuco1/org-clock-budget"))
        ;; (org-timeblock :location (recipe :fetcher github :repo "ichernyshovvv/org-timeblock"))
        ;; titlecase
        ;; (pdftotext :location (recipe :fetcher github :repo "tecosaur/pdftotext.el"
        ;;                              :files ("*.el" "*.org")))
        ;; (orgdiff :location (recipe :fetcher github :repo "tecosaur/orgdiff"
        ;;                            :files ("*.el" "*.org")))
        ;; (org-link-base :location (recipe
        ;;                           :fetcher github :repo "cashpw/org-link-base"))
        ;; (edraw-org
        ;;  :location (recipe :fetcher github :repo "misohena/el-easydraw"))
        )
      )
    ```

<!--list-separator-->

2.  Configurations

    ```elisp
    ;;; Configurations

    ;;;; TODO org-noter
    ;; (:name org-noter
    ;;   :after (progn (add-hook 'org-noter-insert-heading-hook
    ;;                   #'org-id-get-create)))

    ;;;; TODO bbdb

    ;; (defun jh-org/init-bbdb ()
    ;;   (use-package bbdb))

    ;;;; TODO Zotero Org Zotxt Inferface

    ;; (use-package zotxt
    ;;   :ensure nil
    ;;   :commands (org-zotxt-insert-reference-link
    ;;              org-zotxt-open-attachment
    ;;              rg-zotxt-update-reference-link-at-point)
    ;;   :config
    ;;   (add-hook 'org-mode #'org-zotxt-mode))

    ;;;; TODO Bibtex-capf

    ;; (use-package bibtex-capf
    ;;   :hook ((org-mode markdown-mode tex-mode latex-mode reftex-mode) . bibtex-capf-mode)
    ;;   :config
    ;;   (setq bibtex-capf-bibliography '("~/Work/bibfile.bib")))

    ;;;; org-reverse-datetree

    (defun jh-org/init-org-reverse-datetree ()
      (use-package org-reverse-datetree
        :after org
        :demand t
        :commands (org-datetree-refile)
        :init
        (setq-default org-reverse-datetree-level-formats '("%Y" "%Y-%m %B" "%Y W%W" "%Y-%m-%d %A"))
        )
      )

    ;;;; 'Load' org-mode.el and my customs

    ;;;;; load org-mode.el

    (defun jh-org/post-init-org ()

      (global-unset-key (kbd "<f6>"))
      (global-unset-key (kbd "<f9>"))

    ;;;;; user-org-directory

      (if (boundp 'user-org-directory)
          (setq org-directory user-org-directory)
        (setq org-directory "~/org/"))

      (message "`org-directory' has been set to: %s" org-directory)

    ;;;;; load org-mode.el

      ;; (load-file (concat dotspacemacs-directory "layers/jh-org/org-mode-crate.el"))
      ;; (load-file (concat dotspacemacs-directory "layers/jh-org/org-mode-jh.el"))
      (load-file (concat dotspacemacs-directory "layers/jh-org/org-mode-a8dff4a.el"))

    ;;;;; fix path

      ;; The following setting is different from the document so that you
      ;; can override the document org-agenda-files by setting your
      ;; org-agenda-files in the variable org-user-agenda-files
      (if (boundp 'org-user-agenda-files)
          (setq org-agenda-files org-user-agenda-files)
        (setq org-agenda-files (quote ("~/org/refile.org"))))

      (if (boundp 'org-user-agenda-diary-file)
          (setq org-agenda-diary-file org-user-agenda-diary-file)
        (setq org-agenda-diary-file "~/org/diary.org"))
      (setq diary-file org-agenda-diary-file)

      (setq plantuml-jar-path "/usr/share/plantuml/plantuml.jar"
            org-plantuml-jar-path "/usr/share/plantuml/plantuml.jar")

      ;; sudo apt-get install ditaa
      (setq org-ditaa-jar-path "/usr/share/ditaa/ditaa.jar")

      (setq org-clock-sound (concat dotspacemacs-directory "assets/sounds/meditation_bell.wav"))
      (setq org-crypt-key "B5ADD9F47612A9DB") ; junghanacs

      (message "Press `C-c a' to get started with your agenda...")

    ;;;;; todo keywords

      ;; keys mentioned in brackets are hot-keys for the States
      ;; ! indicates insert timestamp
      ;; @ indicates insert note
      ;; / indicates entering the state
      (setq org-todo-keywords
            (quote ((sequence "TODO(t)" "NEXT(n)" "|" "DONE(d)")
                    (sequence "WAITING(w@/!)" "HOLD(h@/!)" "|" "DONT(o)" "CANCELLED(c@/!)" "PHONE(p)" "MEETING(m)")
                    (sequence "NOTE(N)" "KLUDGE(K)" "DEPRECATED(D)" "TEMP(T)" "REVIEW (R)" "FIXME(F)"))))

      (defface my/org-bold-todo '((t :inherit (bold org-todo))) "Face for bold TODO-type Org keywords.")
      (defface my/org-bold-done '((t :inherit (bold org-done))) "Face for bold DONE-type Org keywords.")
      (defface my/org-bold-next '((t :inherit (bold org-todo) :foreground "royal blue" )) "Face for bold NEXT-type Org keywords.")
      (defface my/org-bold-shadow '((t :inherit (bold shadow))) "Face for bold and `shadow' Org keywords.")
      (defface my/org-todo-special '((t :inherit (font-lock-keyword-face bold org-todo))) "Face for special TODO-type Org keywords.")
      (setq org-todo-keyword-faces
            '(("TODO" . my/org-bold-todo)
              ("FIXME" . my/org-bold-todo)

              ("NEXT" . my/org-bold-next)

              ("DONE" . my/org-bold-done)
              ("FIXED" . my/org-bold-done)
              ("CANCELLED" . my/org-bold-done)
              ("DEPRECATED" . my/org-bold-done)

              ("DONT" . my/org-bold-shadow)
              ("WAITING" . my/org-bold-shadow)
              ("HOLD" . my/org-bold-shadow)

              ("MEETING" . my/org-todo-special)
              ("PHONE" . my/org-todo-special)
              ("NOTE" . my/org-todo-special)
              ("KLUDGE" . my/org-todo-special)
              ("TEMP" . my/org-todo-special)
              ("REVIEW" . my/org-todo-special)
              ))

      ;; (setq org-use-fast-todo-selection t) ; default auto
      ;; (setq org-use-fast-tag-selection t) ; default auto

      (setq org-todo-state-tags-triggers
            (quote (("CANCELLED" ("CANCELLED" . t))
                    ("WAITING" ("WAITING" . t) ("NEXT"))
                    ("HOLD" ("WAITING") ("HOLD" . t))
                    (done ("WAITING") ("HOLD") ("NEXT"))
                    ("TODO" ("WAITING") ("CANCELLED") ("HOLD") ("NEXT"))
                    ("NEXT" ("WAITING") ("CANCELLED") ("HOLD") ("NEXT" . t))
                    ("DONE" ("WAITING") ("CANCELLED") ("HOLD") ("NEXT"))
                    )))

      (setq org-priority-faces '((?A . error) (?B . warning) (?C . success)))

    ;;;;; fnotify
      ;; 22/10/11--22:18 :: headline 설정 좋다.
      (setq org-fontify-todo-headline nil)
      ;; done 해드라인 폰트 변경을 하지 않는다. 색상 때문에 doom theme 변경시 제대로 안 보임
      (setq org-fontify-done-headline nil)
      (setq org-fontify-whole-heading-line t)

      ;; quote 와 verse block 도 배경 색상을 바꾼다
      (setq org-fontify-quote-and-verse-blocks t)

    ;;;;; shift

      ;; Shift 거슬리는 것을 막아주는 아주 요긴한 설정이다.
      (setq org-treat-S-cursor-todo-selection-as-state-change nil)

      (setq org-support-shift-select nil) ; default nil
      (setq shift-select-mode nil) ; default t

    ;;;;; imenu ellipsis bookmark

      ;; Search on https://www.compart.com/en/unicode/U+25BF
      ;; Unicode Character “◉” (U+25C9)
      ;; Unicode Character “▾” (U+25BE)
      (setq org-imenu-depth 4) ; default 2
      (setq org-ellipsis " ◉") ;; "…"
      (setq org-capture-bookmark nil)

    ;;;;; pretty-entities / bullet lists / image-width

      (setq org-image-actual-width (min (/ (display-pixel-width) 3) 640))

      ;; Org styling, hide markup etc. 테스트
      ;; 왜 minemacs 는 org-pretty 설정을 둘다 t 로 했을까?  org-pretty-entities 가
      ;; 설정되면 abc_def 에서 def 가 아래로 기어 들어간다.
      (setq org-pretty-entities nil) ; very important
      ;; orgmode 익스포트 할 때, underscore 가 subscripts 변환 방지
      ;; http://ohyecloudy.com/emacsian/2019/01/12/org-export-with-sub-superscripts/
      (setq org-pretty-entities-include-sub-superscripts nil)

      ;; Replace two consecutive hyphens with the em-dash
      (add-hook 'org-mode-hook (lambda ()
                                 (push '("---" . "—") prettify-symbols-alist)
                                 (push '("->" . "⟶" ) prettify-symbols-alist)
                                 (push '("=>" . "⟹") prettify-symbols-alist)
                                 (prettify-symbols-mode)))

      ;; Use utf-8 bullets for bullet lists -- this isn't great, but a bit nicer than nothing.
      ;; Ideally should use monospace font for spaces before bullet item, and use different bullets by list level.
      (font-lock-add-keywords 'org-mode
                              '(("^ *\\([+]\\) "
                                 (0 (prog1 () (compose-region (match-beginning 1) (match-end 1) "•"))))))
      (font-lock-add-keywords 'org-mode
                              '(("^ *\\([-]\\) "
                                 (0 (prog1 () (compose-region (match-beginning 1) (match-end 1) "◦"))))))

    ;;;;; element-cache

      ;; The new org-data element provides properties from top-level property drawer,
      ;; buffer-global category, and :path property containing file path for file Org buffers.
      (setq org-element-use-cache nil) ; default t
      ;; Element cache persists across Emacs sessions
      (setq org-element-cache-persistent nil) ; default t

    ;;;;; multi-byte

      ;; 22/10/12--15:49 :: 멀티 바이트 강조
      ;; https://github.com/clockoon/my-emacs-setting/blob/master/config.org
      ;; org-mode 는 기본적으로 강조문(굵게, 이탤릭 등)을 하나의 단어에
      ;; 대해서만 적용하도록 하고 있습니다. 예컨대 *이렇게*는 굵게 글씨를
      ;; 쓸 수 없습니다. 조사가 들어가는 한중일 언어에 쓰기에는 부적절한
      ;; 정책입니다. 따라서 강조문자 양 옆에 (알파벳이 아닌) 멀티바이트
      ;; 문자가 오더라도 작동하도록 설정을 변경합니다(물론 이는 완전한
      ;; 해결책은 아니며, 더 합리적인 방법에 대해서는 고민이 필요합니다.
      (setcar org-emphasis-regexp-components
              " \t('\"{[:multibyte:]")
      (setcar (nthcdr 1 org-emphasis-regexp-components)
              "[:multibyte:]- \t.,:!?;'\")}\\")
      (org-set-emph-re 'org-emphasis-regexp-components
                       org-emphasis-regexp-components)

      ;; 한자 옆에서도 강조가 되도록
      ;; (org-set-emph-re 'org-emphasis-regexp-components
      ;;                  (let ((cjk "[:nonascii:]")) ;; 应该使用 \\cc\\cj\\ch 但 char alternates 不支持 category 所以只能用 char class.
      ;;                    (pcase-let ((`(,f ,s . ,r) org-emphasis-regexp-components))
      ;;                      `(,(concat f cjk) ,(concat s cjk) . ,r)
      ;;                      )
      ;;                    ))

    ;;;;; org-hide

      ;; Hide ~*~, ~~~ and ~/~ in org text.
      ;; org-indent-mode 사용하면 org-hide-leading-starts 자동 on
      ;; Org styling, hide markup etc. = / ~
      (setq org-hide-emphasis-markers t) ; work with org-appear
      (setq org-hide-block-startup nil)
      (setq org-hide-macro-markers nil)

    ;;;;; org-startup-folded

      ;; fold / overview  - collapse everything, show only level 1 headlines
      ;; content          - show only headlines
      ;; nofold / showall - expand all headlines except the ones with :archive:
      ;;                    tag and property drawers
      ;; showeverything   - same as above but without exceptions
      ;; #+STARTUP: fold 를 기본값으로 한다. org 파일을 열었을 때, overview 를 가장 먼저 보고 싶기 때문
      (setq org-startup-folded 'show2levels)

    ;;;;; org-src

      (setq org-src-tab-acts-natively t)
      (setq org-src-window-setup 'other-window)

      ;; DONT org-block and hide leading stars
      ;; no use for me, I always press this key accidentally
      ;; (unbind-key "C-'" 'org-mode-map)

    ;;;;; org-export

      ;; org-mode 파일에는 관련 설정이 없다. Ahyatt 에서 가져옴
      ;; We also need to make exporting better to work more naturally with the actual Roam research site.
      ;; (setq org-export-with-toc nil) ; default t
      ;; (setq org-export-preserve-breaks t) ; default  nil
      ;; (setq org-export-with-properties t) ; default nil
      ;; (setq org-export-with-tags nil) ; default t

    ;;;;; org-pomodoro

      ;; A pomodoro group is for a day, so after 8 hours of no activity, that's a group.
      (require 'org-pomodoro)
      (setq org-pomodoro-expiry-time (* 60 8))
      (setq org-pomodoro-manual-break t)
      (setq org-pomodoro-play-sounds nil)

      (defun ash/org-pomodoro-til-meeting ()
        "Run a pomodoro until the next 30 minute boundary."
        (interactive)
        (let ((org-pomodoro-length (mod (- 30 (cadr (decode-time (current-time)))) 30)))
          (org-pomodoro)))

    ;;;;; more tuned

      ;; Indentation
      (if window-system
          (setq org-startup-indented t)
        (setq org-startup-indented nil))

      ;; nil 이면 C-c C-o 으로 접근한다.
      (setq org-mouse-1-follows-link t)

      ;; (require 'ox-taskjuggler)
      ;; (add-to-list 'org-export-backends 'taskjuggler)

    ;;;;; TODO org-columns

      ;; WATCH vedang's workflow
      ;; vedang's style from org-mode-crate
      (setq org-columns-default-format
            "%50ITEM(Task) %5Effort(Effort){:} %5CLOCKSUM %3PRIORITY %20DEADLINE %20SCHEDULED %20TIMESTAMP %TODO %CATEGORY(Category) %TAGS")

    ;;;;; org-agenda-log-mode and clock-mode

      ;; Show all agenda dates - even if they are empty
      (setq org-agenda-show-all-dates t)
      (setq org-agenda-start-with-log-mode t)

      ;; Agenda log mode items to display (closed clock : default)
      ;; 이전 이맥스는 state 가 기본이었다. 지금은 시간 기준으로 표기한다.
      ;; closed    Show entries that have been closed on that day.
      ;; clock     Show entries that have received clocked time on that day.
      ;; state     Show all logged state changes.
      ;; (setq org-agenda-log-mode-items '(closed clock state))
      (setq org-agenda-log-mode-add-notes nil)

      ;; sort 관련 기능을 확인해보고 정의한 함수들이 필요 없으면 빼면 된다.
      (setq org-agenda-sort-notime-is-late t) ; Org 9.4
      (setq org-agenda-sort-noeffort-is-high t) ; Org 9.4

      ;; Time Clocking
      (setq org-clock-idle-time 30) ; 10
      (setq org-clock-reminder-timer (run-with-timer
                                      t (* org-clock-idle-time 20) ; 60
                                      (lambda ()
                                        (unless (org-clocking-p)
                                          (alert "Do you forget to clock-in?"
                                                 :title "Org Clock")))))
      (org-clock-auto-clockout-insinuate) ; auto-clockout
      ;; modeline 에 보이는 org clock 정보가 너무 길어서 줄임
      (setq org-clock-string-limit 30) ; default 0
      (setq org-clock-history-length 10) ;;
      ;; org-clock-persist for share with machines
      (setq org-clock-persist-query-save t)
      (setq org-clock-persist-query-resume t)

      ;; current  Only the time in the current instance of the clock
      ;; today    All time clocked into this task today
      ;; repeat   All time clocked into this task since last repeat
      ;; all      All time ever recorded for this task
      ;; auto     Automatically, either all, or repeat for repeating tasks
      (setq org-clock-mode-line-entry t)
      (setq org-clock-mode-line-line-total 'auto) ; default nil

    ;;;;; org-tag and category

      (setq org-auto-align-tags nil) ; default t
      (setq org-tags-column 0) ; default -77
      (setq org-agenda-tags-column -80) ;; 'auto ; org-tags-column

      (setq org-agenda-show-inherited-tags nil)

      (setq org-tag-alist (quote ((:startgroup)
                                  ("@errand" . ?e)
                                  ("@office" . ?o)
                                  ("@home" . ?H)
                                  ("@farm" . ?f)
                                  (:endgroup)
                                  ("WAITING" . ?w)
                                  ("IMPORTANT" . ?i)
                                  ("NEXT" . ?n)
                                  ("HOLD" . ?h)
                                  ("PERSONAL" . ?P)
                                  ("WORK" . ?W)
                                  ("FARM" . ?F)
                                  ("ORG" . ?O)
                                  ("crypt" . ?E)
                                  ("NOTE" . ?N)
                                  ("CANCELLED" . ?c)
                                  ("FLAGGED" . ??))))

      (add-to-list 'org-tags-exclude-from-inheritance "project")

    ;;;;; org-agenda-custom-commands

      (add-to-list 'org-modules 'org-habit)
      (add-to-list 'org-modules 'ol-man)

      (setq org-agenda-prefix-format
            '((agenda  . " %i %-14:c%?-12t% s")
              (todo  . " %i %-14:c")
              (tags  . " %i %-14:c")
              (search . " %i %-14:c")))

      (setq org-agenda-hide-tags-regexp
            "agenda\\|LOG\\|ATTACH\\|GENERAL\\|BIRTHDAY\\|PERSONAL\\|PROFESSIONAL\\|TRAVEL\\|PEOPLE\\|HOME\\|FINANCE\\|PURCHASES")

      (add-hook 'org-agenda-finalize-hook
                (lambda ()
                  ;; (setq-local line-spacing 0.2)
                  (define-key org-agenda-mode-map
                              [(double-mouse-1)] 'org-agenda-goto-mouse)))

      (defun cc/org-agenda-goto-now ()
        "Redo agenda view and move point to current time '← now'"
        (interactive)
        (org-agenda-redo)
        (org-agenda-goto-today)

        (if window-system
            (search-forward "← now ─")
          (search-forward "now -"))
        )

      (add-hook 'org-agenda-mode-hook
                (lambda ()
                  (define-key org-agenda-mode-map (kbd "<f2>") 'org-save-all-org-buffers)
                  (define-key org-agenda-mode-map (kbd "M-p") 'org-pomodoro)
                  (define-key org-agenda-mode-map (kbd "M-.") 'cc/org-agenda-goto-now)))

    ;;;;; hook

      ;; spacemacs style
      (add-hook 'org-mode-hook
                (lambda ()
                  (setq-local org-emphasis-alist '(("*" bold)
                                                   ("/" italic)
                                                   ("_" underline)
                                                   ("=" org-verbatim verbatim)
                                                   ("~" org-kbd)
                                                   ("+" (:strike-through t))))))
      ;; (remove-hook 'org-capture-mode-hook 'spacemacs//org-capture-start) ;; back to default

      (advice-add 'org-archive :after 'org-save-all-org-buffers)
      ;; (add-hook 'org-capture-after-finalize-hook 'org-save-all-org-buffers)

      (add-hook 'org-mode-hook 'visual-line-mode)
      (add-hook 'org-mode-hook 'org-indent-mode)
      ;; (add-hook 'org-mode-hook 'auto-fill-mode) ;; 2023-12-19 conflict ekg-tag

    ;;;;; org-capture-templates -- org-refile-file

      ;; Capture templates for: TODO tasks, Notes, appointments, phone calls, meetings, and org-protocol
      (setq org-capture-templates
            (quote (("t" "todo" entry (file org-refile-file)
                     "* TODO [#C] %?\n%U\n%a\n" :clock-in t :clock-resume t)
                    ("r" "respond" entry (file org-refile-file)
                     "* NEXT Respond to %:from on %:subject\nSCHEDULED: %t\n%U\n%a\n" :clock-in t :clock-resume t :immediate-finish t)
                    ("n" "note" entry (file org-refile-file)
                     "* %? :NOTE:\n%U\n%a\n" :clock-in t :clock-resume t)
                    ("w" "org-protocol" entry (file org-refile-file)
                     "* TODO Review %c\n%U\n" :immediate-finish t)
                    ("m" "Meeting" entry (file org-refile-file)
                     "* MEETING with %? :MEETING:\n%U" :clock-in t :clock-resume t)
                    ("h" "Phone call" entry (file org-refile-file)
                     "* PHONE %? :PHONE:\n%U" :clock-in t :clock-resume t)
                    ("H" "Habit" entry (file org-refile-file)
                     "* NEXT %?\n%U\n%a\nSCHEDULED: %(format-time-string \"%<<%Y-%m-%d %a .+1d/3d>>\")\n:PROPERTIES:\n:STYLE: habit\n:REPEAT_TO_STATE: NEXT\n:END:\n"))))

      ;; ("f" "Fleeting note (/w Clock)" entry (file+headline org-refile-file "Slipbox")
      ;;   "* TODO %^{Note title}\nContext: %U\n%a\n%?" :clock-in t :clock-resume t)
      ;; Fleeting Note
      ;; (push `("f" "Fleeting note" item
      ;;          (file+headline org-refile-file "Notes")
      ;;          "+ %U %?" :clock-in t :clock-resume t)
      ;;   org-capture-templates)

      ;; One-click Capture for Tasks. Captures the task immediately and gets out of your way.
      (push `("T" "Todo Immediate Finish" entry
              (file+headline org-refile-file "FleetBox")
              "* TODO [#C] %^{Todo title}\n%t\n%a\n%?"
              ;; :clock-in t :clock-resume t
              :immediate-finish t)
            org-capture-templates)

    ;;;;; org-capture-templates -- org-iam-file

    ;;;;; org-capture-templates -- org-contact-file

      (push `("c" "Contacts" entry (file org-contact-file)
              "* %(org-contacts-template-name)
      :PROPERTIES:
      :GITHUB:
      :EMAIL:
      :URL:
      :NOTE:
      :END:\n%U\n%T\n%a\n") org-capture-templates)


    ;;;;; org-capture-templates -- org-links-file

      (push `("l" "links" plain (file+function org-links-file org-capture-goto-link)
              "%i\n%U\n%T\n%a\n" :empty-lines 1 :immediate-finish t)
            org-capture-templates)

    ;;;;; org-capture-templates -- org-log-file with org-reverse-datetree

      (require 'org-reverse-datetree)
      (setq org-agenda-bulk-custom-functions '((?R org-datetree-refile)))
      (defun org-datetree-refile ()
        (interactive)
        (org-reverse-datetree-refile-to-file org-log-file))

      (push `("j" "Journal"
              entry (file+function org-log-file org-reverse-datetree-goto-date-in-file)
              "* %<%H:%M> - %?\n%U\n%a\n" :clock-in t :clock-resume t)
            org-capture-templates)
      ;; :empty-lines 1 :prepend t -- 역순 등록

      ;; Capture some feedback for myself or a quick check-in, which I will into other
      ;; more refined notes later. 나 자신을 위한 피드백이나 간단한 점검 사항을 기록해
      ;; 두었다가 나중에 좀 더 세련된 노트로 정리할 수 있습니다.
      (push `("S" "The Start of Day Planning Routine" entry
              (file+function org-log-file org-reverse-datetree-goto-date-in-file)
              (file ,(expand-file-name (concat org-directory "capture-templates/workday.start.org")))
              :prepent t :clock-in t :clock-resume t :empty-lines 1)
            org-capture-templates)

      (push `("E" "The End of Day Reflection Routine" entry
              (file+function org-log-file org-reverse-datetree-goto-date-in-file)
              (file ,(expand-file-name (concat org-directory "capture-templates/workday.end.org")))
              :prepend nil :clock-in t :clock-resume t :empty-lines 1)
            org-capture-templates)

      ;; 리뷰 프로세스를 어떻게 할 것인가?
      (push `("R" "Review") org-capture-templates)
      (push `("Ry" "Yesterday" plain
              (file+function org-log-file
                             (lambda () (org-reverse-datetree-goto-date-in-file
                                         (time-add (current-time) (days-to-time -1)))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("Rt" "Today" plain
              (file+function org-log-file
                             (lambda () (org-reverse-datetree-goto-date-in-file)))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("Rl" "Last Week" plain
              (file+function org-log-file
                             (lambda () (let ((org-reverse-datetree-level-formats
                                               (butlast org-reverse-datetree-level-formats)))
                                          (org-reverse-datetree-goto-date-in-file
                                           (time-add (current-time) (days-to-time -7))))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("Rw" "This Week" plain
              (file+function org-log-file
                             (lambda () (let ((org-reverse-datetree-level-formats
                                               (butlast org-reverse-datetree-level-formats)))
                                          (org-reverse-datetree-goto-date-in-file))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("RD" "Select a Date" plain
              (file+function org-log-file
                             org-reverse-datetree-goto-read-date-in-file)
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("RW" "Select a Week" plain
              (file+function org-log-file
                             (lambda () (let ((org-reverse-datetree-level-formats
                                               (butlast org-reverse-datetree-level-formats)))
                                          (org-reverse-datetree-goto-read-date-in-file))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("RM" "Select a Month" plain
              (file+function org-log-file
                             (lambda () (let ((org-reverse-datetree-level-formats
                                               (butlast org-reverse-datetree-level-formats 2)))
                                          (org-reverse-datetree-goto-read-date-in-file))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)
      (push `("RY" "Select a Year" plain
              (file+function org-log-file
                             (lambda () (let ((org-reverse-datetree-level-formats
                                               (butlast org-reverse-datetree-level-formats 3)))
                                          (org-reverse-datetree-goto-read-date-in-file))))
              "%?\n%i\n" :immediate-finish t :jump-to-captured t)
            org-capture-templates)

    ;;;;; end-of defun
      ) ;; end-of defun

    ;;;;; DONT org-appear

    ;; Disable org-appear for terminal-mode
    ;; 'always' means that elements are toggled every time they are under the cursor.
    ;; 'manual' means that toggling starts on call to org-appear-manual-start
    ;; 'on-change' means that elements are toggled only when the buffer is modified
    ;; or when the element under the cursor is clicked with a mouse.
    ;; (setq org-appear-trigger 'on-change) ; 'manual
    ;; (setq org-appear-autolinks nil)

    ;;;; org-gcal

    (defun jh-org/init-org-gcal ()
      (use-package org-gcal
        :after org
        :defer 7
        :init
        (setq oauth2-auto-plstore (concat user-emacs-directory "oauth2-auto.plstore"))
        (setq plstore-cache-passphrase-for-symmetric-encryption t)
        (setq epg-pinentry-mode 'loopback)
        (setenv "GPG_AGENT_INFO")
        ;; (setq org-gcal-remove-api-cancelled-event t) ;; delete removed events without asking.
        (setq org-gcal-client-id "1045932772216-sifrvrrq4oqpoaalmi9r2q2cuaam4to9.apps.googleusercontent.com"
              org-gcal-client-secret "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"
              org-gcal-header-alist
              '(("junghanacs@gmail.com" . "#+PROPERTY: TIMELINE_FACE \"pink\"\n")
                ("e07727dc2c9e2a565eb162c45cfd31796acefc04de10540cb84a439de2fabe54@group.calendar.google.com" . "#+PROPERTY: TIMELINE_FACE \"#8ae234\"\n"))
              org-gcal-file-alist
              '(("junghanacs@gmail.com" .  "~/sync/org/roam/workflow/gcal-office.org")
                ("e07727dc2c9e2a565eb162c45cfd31796acefc04de10540cb84a439de2fabe54@group.calendar.google.com" . "~/sync/org/roam/workflow/gcal-home.org"))
              org-gcal-auto-archive nil
              org-gcal-notify-p nil)
        ;; (spacemacs/set-leader-keys
        ;;   "aoS" 'org-gcal-sync)
        :config
        (org-gcal-reload-client-id-secret)
        ;; (add-hook 'org-save-all-org-buffers (lambda () (org-gcal-sync) ))

        ;; Added to stop org-agenda from freezing after sync is locked
        ;; (add-hook 'org-agenda-mode-hook (lambda () (org-gcal--sync-unlock)) 100)
        ))

    ;;;; Utility

    ;;;;; org-remoteimg

    (defun jh-org/init-org-remoteimg ()
      (use-package org-remoteimg
        :if window-system
        :after org
        :init
        ;; optional: set this to wherever you want the cache to be stored
        (setq url-cache-directory "~/.cache/emacs/url")
        (setq org-display-remote-inline-images 'cache) ;; enable caching
        ;; or this if you don't want caching
        ;; (setq org-display-remote-inline-images 'download)
        ;; or this if you want to disable this plugin
        ;; (setq org-display-remote-inline-images 'skip)
        ;; this is a emacs built-in feature
        (setq url-automatic-caching t) ; default nil
        ;; (setq url-cache-expire-time 7200)
        ))

    (defun jh-org/init-org-imgtog ()
      (use-package org-imgtog
        :if window-system
        :after org
        :init
        ;; (add-hook 'org-mode-hook 'org-imgtog-mode)
        (setq org-imgtog-preview-delay 0.5) ;; wait 0.5 seconds before toggling
        (setq org-imgtog-preview-delay-only-remote t) ;; only delay for remote images
        ))

    ;;;;; orglink

    (defun jh-org/init-orglink ()
      (use-package orglink
        :after org
        :init
        (add-hook 'spacemacs-post-user-config-hook #'global-orglink-mode)
        ))

    ;;;;; org-remark

    (defun jh-org/init-org-remark ()
      (use-package org-remark
        :after org
        :demand t
        :config
        (use-package org-remark-info :after info :config (org-remark-info-mode +1))
        (use-package org-remark-eww  :after eww  :config (org-remark-eww-mode +1))
        (use-package org-remark-nov  :after nov  :config (org-remark-nov-mode +1))
        (setq org-remark-notes-file-name (concat org-notes-directory "20231111T094444==03--org-remark__annotate.org"))

        ;; It is recommended that `org-remark-global-tracking-mode' be
        ;; enabled when Emacs initializes. Alternatively, you can put it to
        ;; `after-init-hook' as in the comment above
        (org-remark-global-tracking-mode +1)
        )
      )

    ;;;;; remember

    (defun jh-org/init-remember ()
      (use-package remember
        :commands remember
        :config
        (setq remember-data-file (concat org-roam-directory "notes/remember_notes.org")
              remember-notes-initial-major-mode 'org-mode
              remember-notes-auto-save-visited-file-name t)
        )
      )

    ;;;;; org-attach

    (defun jh-org/init-org-attach ()
      (use-package org-attach
        :after org
        :commands (org-attach-follow org-attach-complete-link)
        :init
        (org-link-set-parameters "attachment"
                                 :follow #'org-attach-follow
                                 :complete #'org-attach-complete-link)
        :config
        (setq org-attach-archive-delete 'query
              ;; org-attach-id-dir (concat org-directory "/attach/")
              org-attach-id-dir "attach/"
              org-attach-method 'mv
              org-attach-store-link-p 'file))
      )

    ;; (global-set-key (kbd "<f1>")
    ;;                 (lambda ()
    ;;                   (interactive)
    ;;                   (consult-org-heading nil '("~/sync/org/roam/workflow/inbox.org"))))

    ;;;;; orgabilize

    (defun jh-org/init-orgabilize ()
      (use-package orgabilize :ensure t :defer 5)
      )

    ;;;;; wikinforg

    (defun jh-org/init-wikinfo () (use-package wikinfo :ensure t))
    ;; wikinforg - Insert the result of a wikinfo search as an Org entry or item.
    ;; wikinforg-capture - Similar to above, but designed with org-capture in mind.
    (defun jh-org/init-wikinforg () (use-package wikinforg :defer 5))

    ;;;; org-download

    (defun jh-org/post-init-org-download ()
      ;; (use-package org-download
      ;;   :after org
      ;;   :commands (org-download-dnd org-download-dnd-base64)
      ;;   :init
      ;;   ;; (add-hook 'dired-mode-hook 'org-download-enable)
      ;;   (unless (eq (cdr (assoc "^\\(https?\\|ftp\\|file\\|nfs\\):" dnd-protocol-alist))
      ;;               'org-download-dnd)
      ;;     (setq dnd-protocol-alist
      ;;           `(("^\\(https?\\|ftp\\|file\\|nfs\\):" . org-download-dnd)
      ;;             ("^data:" . org-download-dnd-base64)
      ;;             ,@dnd-protocol-alist)))
      ;;   :config

      (setq org-download-display-inline-images nil)
      (setq org-download-annotate-function (lambda (_link) "")
            org-download-method 'attach
            ;; org-download-screenshot-method "screencapture -i %s"
            )
      (setq org-download-image-attr-list
            '("#+attr_html: :width 100% :align center"
              "#+caption: "
              "#+attr_org: :width 800px"))
      (setq org-download-timestamp"%Y%m%d_%H%M%S_")
      )

    ;;;;; DONT org-web-tools

    ;; (defun jh-org/init-org-web-tools ()
    ;;   (use-package org-web-tools
    ;;     :after org
    ;;     :config
    ;;     ;; (require 'org-protocol-capture-html)
    ;;     ;; 클립보드에 복사 된 url 을 org 로 가져온다. footnote 는 개선 되야 한다.

    ;;     (defun org-web-tools--convert-fns-relative ()
    ;;       "Convert ^{n} format footnotes in document to org syntax."
    ;;       (interactive)
    ;;       (save-match-data
    ;;         (while (re-search-forward "\\^{\\([[:digit:]]+\\)}" nil t)
    ;;           (let ((match (match-string 1)))
    ;;             (replace-match (format "[fn:%s]" match))))))

    ;;     (defun org-web-tools--convert-fns-relative-alt ()
    ;;       "Convert [[#enN]][N]] format footnotes in document to org syntax."
    ;;       (interactive)
    ;;       (save-match-data
    ;;         (while (re-search-forward "\\[\\[#\\(en\\|fn\\)\\([[:digit:]]+\\)\\]\\[[[:digit:]\\|↩]+\\]\\]" nil t)
    ;;           ;; NB: 2 here not 1! cd also use (or) and test for first group containing digits
    ;;           (let ((match (match-string 2))
    ;;                  (match-type (match-string 1)))
    ;;             (replace-match (format "[fn:%s]" match))
    ;;             ;; org-fns must be at bol to work:
    ;;             (when (and (equal match-type "fn") ;only for fns in footnotes section
    ;;                     (not (bolp)))
    ;;               (backward-sexp) ; move point to before org fn's "["
    ;;               (kill-line -0)))))) ; kill backward to bol
    ;;     ))

    ;;;;; org-pandoc-import

    (defun jh-org/init-org-pandoc-import ()
      (use-package org-pandoc-import
        :defer 10))

    ;;;;; org-make-toc

    (defun jh-org/init-org-make-toc ()
      (use-package org-make-toc :defer 10))
    ;; (add-hook 'org-mode-hook 'org-make-toc-mode) ; 수동으로 호출하자.

    ;; (defun jh-org/init-toc-org ()
    ;;   (use-package toc-org () :defer t))

    ;; (if (require 'toc-org nil t)
    ;;     (progn
    ;;       (setq toc-org-max-depth 5)
    ;;       ;; (add-hook 'org-mode-hook 'toc-org-mode)
    ;;       ;; (add-hook 'markdown-mode-hook 'toc-org-mode)
    ;;       ;; (define-key markdown-mode-map (kbd "\C-c\C-o") 'toc-org-markdown-follow-thing-at-point)
    ;;       )
    ;;   (warn "toc-org not found"))

    ;;;;; ob-translate

    (defun jh-org/init-ob-translate ()
      (use-package ob-translate
        :defer 10
        :config
        (setq ob-translate:default-dest "ko")))

    ;;;;; ob-racket

    (defun jh-org/init-ob-racket ()
      (use-package ob-racket))

    ;;;;; ob-mermaid

    ;; sudo npm install -g @mermaid-js/mermaid-cli
    (defun jh-org/init-ob-mermaid ()
      (use-package ob-mermaid :defer 7))

    ;;;;; ob-d2

    ;; https://github.com/terrastruct/d2
    (defun jh-org/init-ob-d2 ()
      (use-package ob-d2
        :defer 6 :init (setq ob-d2-command "~/.local/bin/d2")))

    ;;;; Math math-preview

    ;; sudo npm install -g git+https://gitlab.com/matsievskiysv/math-preview

    (defun jh-org/init-math-preview ()
      (use-package math-preview
        :defer 1
        :commands math-preview-all math-preview-clear-all
        ;; :hook (find-file . (lambda ()
        ;;                      (when (eq major-mode 'org-mode)
        ;;                        (auto/math-preview-all))))
        :config
        ;; (setq math-preview-scale 1.1)
        ;; (setq math-preview-raise 0.2)
        ;; (setq math-preview-margin '(1 . 0))
        (add-to-list 'org-options-keywords "NO_MATH_PREVIEW:")))

    ;;;;; More

    ;; jousimies-dotfiles/lisp/init-org+.el:74

    ;; (use-package org-download
    ;;   :bind (("C-c d c" . org-download-clipboard)
    ;;           ("C-c d y" . org-download-yank)
    ;;           ("C-c d s" . org-download-screenshot)
    ;;           ("C-c d r" . org-download-rename-at-point)
    ;;           ("s-v" . my/yank))
    ;;   :init
    ;;   (setq org-download-image-dir (expand-file-name "pictures" my-galaxy))
    ;;   (setq org-download-heading-lvl nil)
    ;;   :config
    ;;   (setq org-download-screenshot-method "screencapture -i %s")
    ;;   (setq org-download-abbreviate-filename-function 'expand-file-name)
    ;;   (setq org-download-timestamp "%Y%m%d%H%M%S")
    ;;   (setq org-download-display-inline-images nil)
    ;;   (setq org-download-annotate-function (lambda (_link) ""))
    ;;   (setq org-download-image-attr-list '("#+NAME: fig: "
    ;;                                         "#+CAPTION: "
    ;;                                         "#+ATTR_ORG: :width 500px"
    ;;                                         "#+ATTR_LATEX: :width 10cm :placement [!htpb]"
    ;;                                         "#+ATTR_HTML: :width 600px"))

    ;;   (defun my/org-download-rename (arg)
    ;;     (interactive "P")
    ;;     (if arg
    ;;       (org-download-rename-last-file)
    ;;       (org-download-rename-at-point)))

    ;;   (defun my/org-download-adjust (&optional basename)
    ;;     "Adjust the last downloaded file.

    ;;   This function renames the last downloaded file, replaces all occurrences of the old file name with the new file name in the Org mode buffer, and updates the CAPTION and NAME headers in the Org mode buffer. "
    ;;     (interactive)
    ;;     (let* ((dir-path (org-download--dir))
    ;;             (newname (read-string "Rename last file to: " (file-name-base org-download-path-last-file)))
    ;;             (ext (file-name-extension org-download-path-last-file))
    ;;             (newpath (concat dir-path "/" newname "." ext)))
    ;;       (when org-download-path-last-file
    ;;         (rename-file org-download-path-last-file newpath 1)
    ;;         (org-download-replace-all
    ;;           (file-name-nondirectory org-download-path-last-file)
    ;;           (concat newname "." ext))
    ;;         (setq org-download-path-last-file newpath))
    ;;       (save-excursion
    ;;         (previous-line 7)
    ;;         (while (re-search-forward "^\\#\\+NAME: fig:" nil t 1)
    ;;           (move-end-of-line 1)
    ;;           (insert newname))
    ;;         (while (re-search-forward "^\\#\\+CAPTION:" nil t 1)
    ;;           (move-end-of-line 1)
    ;;           (insert newname))
    ;;         (while (re-search-forward (expand-file-name "~") nil t 1)
    ;;           (replace-match "~" t nil)))))

    ;;   (advice-add 'org-download-clipboard :after 'my/org-download-adjust)

    ;;   (defun my/clipboard-has-image-p ()
    ;;     (let ((clipboard-contents (shell-command-to-string "pbpaste")))
    ;;       (string-match-p "\\(\\.jpeg\\|\\.jpg\\|\\.png\\)$" clipboard-contents)))

    ;;   (defun my/yank ()
    ;;     (interactive)
    ;;     (if (my/clipboard-has-image-p)
    ;;       (org-download-clipboard)
    ;;       (cond ((eq major-mode 'vterm-mode) (term-paste))
    ;;         (t (yank))))))

    ;; (defun org-export-docx ()
    ;;   "Convert org to docx."
    ;;   (interactive)
    ;;   (let ((docx-file (concat (file-name-sans-extension (buffer-file-name)) ".docx"))
    ;;          (template-file (expand-file-name "template/template.docx" user-emacs-directory)))
    ;;     (shell-command (format "pandoc %s -o %s --reference-doc=%s" (buffer-file-name) docx-file template-file))
    ;;     (message "Convert finish: %s" docx-file)))

    ;; https://www.reddit.com/r/emacs/comments/yjobc2/comment/iur16c7/
    ;; (defun nf/parse-headline (x)
    ;;   (plist-get (cadr x) :raw-value))

    ;; (defun nf/get-headlines ()
    ;;   (org-element-map (org-element-parse-buffer) 'headline #'nf/parse-headline))

    ;; (defun nf/link-to-headline ()
    ;;   "Insert an internal link to a headline."
    ;;   (interactive)
    ;;   (let* ((headlines (nf/get-headlines))
    ;;           (choice (completing-read "Headings: " headlines nil t))
    ;;           (desc (read-string "Description: " choice)))
    ;;     (org-insert-link buffer-file-name (concat "*" choice) desc)))

    ;;;; citar

    (defun jh-org/init-citar ()
      (use-package citar
        ;; :hook (org-mode . citar-capf-setup)
        ;; :bind
        ;; (:map org-mode-map :package org ("C-c b" . #'org-cite-insert))
        :init
        (require 'oc)

        (setq config-bibfiles (list "~/sync/org/bib/zotero-biblatex.bib"))
        (setq citar-bibliography config-bibfiles)
        ;; use #+cite_export: csl apa.csl
        (setq org-cite-csl-styles-dir "~/sync/logseq/zotero/styles")
        (setq citar-notes-paths '("~/sync/org/roam/notes/"))


        (setq org-cite-global-bibliography config-bibfiles)
        (setq org-cite-insert-processor 'citar)
        (setq org-cite-follow-processor 'citar)
        (setq org-cite-activate-processor 'citar)
        (setq citar-symbol-separator "  ")

        :config

        (require 'bibtex)
        (setq bibtex-file-path "~/sync/org/bib/"
              bibtex-files '("zotero-biblatex.bib")
              bibtex-notes-path "~/sync/org/roam/notes/"

              bibtex-align-at-equal-sign t
              bibtex-autokey-titleword-separator "-"
              bibtex-autokey-year-title-separator "-"
              bibtex-autokey-name-year-separator "-"
              bibtex-dialect 'biblatex)

        ;; default
        (setq citar-templates
              '((main . "${date year issued:4}  ${author editor:20%sn}  ${title:58}")
                (suffix . "${=type=:12} ${shorttitle:20} ${tags keywords:*}")
                (preview . "${author editor:%etal} (${year issued date}) ${title}, ${journal journaltitle publisher container-title collection-title}.\n")
                (note . "Notes on ${author editor:%etal}, ${title}")))

        (require 'nerd-icons)
        (defvar citar-indicator-files-icons
          (citar-indicator-create
           :symbol (nerd-icons-faicon "nf-fa-file_o" :face 'error)
           :function #'citar-has-files
           :padding "  " ; need this because the default padding is too low for these icons
           :tag "has:files"))

        (defvar citar-indicator-links-icons
          (citar-indicator-create
           :symbol (nerd-icons-mdicon "nf-md-link" :face 'org-link)
           :function #'citar-has-links
           :padding "  "
           :tag "has:links"))

        (defvar citar-indicator-notes-icons
          (citar-indicator-create
           :symbol (nerd-icons-faicon "nf-fa-file_text" :face 'warning)
           :function #'citar-has-notes
           :padding "  "
           :tag "has:notes"))

        (defvar citar-indicator-cited-icons
          (citar-indicator-create
           :symbol (nerd-icons-faicon "nf-fa-circle_o" :face 'info)
           :function #'citar-is-cited
           :padding "  "
           :tag "is:cited"))

        (setq citar-indicators
              (list citar-indicator-files-icons
                    citar-indicator-links-icons
                    citar-indicator-notes-icons
                    citar-indicator-cited-icons))
        )
      )

    ;;;;; citar-embark

    (defun jh-org/init-citar-embark ()
      (use-package citar-embark
        :after citar embark
        :config
        (citar-embark-mode 1)))

    ;;;; org-present

    (defun jh-org/post-init-org-present ()
      (setq org-present-text-scale 2)
      ;; (setq org-present-text-scale 2) ; default 5
      (defun my/org-modern-present-start ()
        (hide-mode-line-mode 1)
        (spacemacs/toggle-fill-column-indicator-off)
        (spacemacs/toggle-line-numbers-off)
        )

      (defun my/org-modern-present-end ()
        (hide-mode-line-mode 0)
        (spacemacs/toggle-fill-column-indicator-on)
        (spacemacs/toggle-line-numbers-on)
        )

      (add-hook 'org-present-mode-hook 'my/org-modern-present-start)
      (add-hook 'org-present-mode-quit-hook 'my/org-modern-present-end)
      )

    ;;;; org-tree-slide

    ;; Simple org outline based presentation mode
    ;; ref: https://github.com/takaxp/org-tree-slide
    (defun jh-org/init-org-tree-slide ()
      (use-package org-tree-slide
        :defer 6
        :ensure t
        ;; :bind (("<f8>" . 'org-tree-slide-mode)
        ;;        ("S-<f8>" . 'org-tree-slide-skip-done-toggle)
        ;;        :map org-tree-slide-mode-map
        ;;        ("<f9>" . 'org-tree-slide-move-previous-tree)
        ;;        ("<f10>" . 'org-tree-slide-move-next-tree)
        ;;        ("<f11>" . 'org-tree-slide-content))
        :hook ((org-tree-slide-play . efs/presentation-setup)
               (org-tree-slide-stop . efs/presentation-end))
        :custom
        (org-tree-slide-slide-in-effect t)
        (org-tree-slide-activate-message "Presentation started!")
        (org-tree-slide-deactivate-message "Presentation finished!")
        (org-tree-slide-header nil) ; t
        (org-tree-slide-breadcrumbs " > ")
        (org-image-actual-width nil)
        :config
        (setq org-tree-slide-skip-outline-level 4) ;; wow!!
        )
      )

    (defun jh-org/init-ox-reveal ()
      (require 'ox-reveal))

    ;;  (setq org-export-html-postamble nil)

    ;;;; org-projectile

    (defun jh-org/post-init-org-projectile ()
      (require 'org-projectile)
      (setq org-project-capture-default-backend
            (make-instance 'org-project-capture-projectile-backend))
      (setq org-project-capture-projects-file org-projectile-file)
      (org-project-capture-single-file)
      (push (org-projectile-project-todo-entry :empty-lines 1)
            org-capture-templates)
      )

    ;;;; 'custom' org-roam

    ;;;;; post-init org-roam
    (defun jh-org/pre-init-org-roam ()
      (spacemacs|use-package-add-hook org-roam
        :pre-init
        (require 'emacsql-sqlite-builtin)
        (setq org-roam-database-connector 'sqlite-builtin))
      )

    ;;;;; custom-org-roam
    (defun jh-org/post-init-org-roam ()
      (require 'org-roam-node)

      (setq org-roam-index-file (concat org-roam-directory "index.org"))

      ;; Navigation in the backlink buffer is intuitive (use RET, C-u RET).
      ;; If org-roam-visit-thing does not work for you, this below might:
      ;; (define-key org-roam-mode-map [mouse-1] #'org-roam-preview-visit)

      ;; dailies directory is set to the Logseq default
      ;; (setq org-roam-dailies-directory "journals/")

      (setq org-roam-file-extensions '("org" "org_archive"))

      (setq org-roam-file-exclude-regexp '("temp/" "daily" "layers/" "reveal-root/" "attach/" "oldseq/" "data/" "archive/" "\\<habits\\.org"))
      (when *is-termux* (add-to-list 'org-roam-file-exclude-regexp "oldnotes/" ))

      ;; https://www.orgroam.com/manual.html#Customizing-Node-Caching
      (setq org-roam-db-node-include-function
            (lambda ()
              (not (member "ATTACH" (org-get-tags)))))

      (setq org-roam-db-gc-threshold most-positive-fixnum)
      (setq org-roam-v2-ack t)

      ;; https://jethrokuan.github.io/org-roam-guide/
      (cl-defmethod org-roam-node-type ((node org-roam-node))
        "Return the TYPE of NODE."
        (condition-case nil
            (file-name-nondirectory
             (directory-file-name
              (file-name-directory
               (file-relative-name (org-roam-node-file node) org-roam-directory))))
          (error "")))

      ;; Codes blow are used to general a hierachy
      ;; for title nodes that under a file
      (cl-defmethod org-roam-node-doom-filetitle ((node org-roam-node))
        "Return the value of \"#+title:\" (if any) from file that NODE resides in.
          If there's no file-level title in the file, return empty string."
        (or (if (= (org-roam-node-level node) 0)
                (org-roam-node-title node)
              (org-roam-get-keyword "TITLE" (org-roam-node-file node)))
    	    ""))
      (cl-defmethod org-roam-node-doom-hierarchy ((node org-roam-node))
        "Return hierarchy for NODE, constructed of its file title, OLP and
    direct title.
            If some elements are missing, they will be stripped out."
        (let ((title     (org-roam-node-title node))
              (olp       (org-roam-node-olp   node))
              (level     (org-roam-node-level node))
              (filetitle (org-roam-node-doom-filetitle node))
              (separator (propertize " > " 'face 'shadow)))
    	  (cl-case level
    	    ;; node is a top-level file
    	    (0 filetitle)
    	    ;; node is a level 1 heading
    	    (1 (concat (propertize filetitle 'face '(shadow)) ; italic
                       separator title))
    	    ;; node is a heading with an arbitrary outline path
    	    (t (concat (propertize filetitle 'face '(shadow)) ; italic
                       separator (propertize (string-join olp " > ")
                                             'face '(shadow)) ; italic
                       separator title)))))

      ;; 모든 새로운 제텔에는 Draft 를 붙인다. HUGO_DRAFT 가 있는데 이게
      ;; 어떻게 활용 될 수 있나? 이미 보낸 글도 수정 할 수 있으니까
      ;; 그때는 draft 라고 하는게 맞겠다.
      ;; 2023-06-22 tempel 로 옮김
      ;; (defun  jethro/tag-new-node-as-draft ()
      ;;   (org-roam-tag-add  '("draft")))
      ;; (add-hook 'org-roam-capture-new-node-hook #'jethro/tag-new-node-as-draft)

      (setq org-roam-node-display-template (concat
                                            (propertize "${type:10} " 'face 'org-checkbox)
                                            (propertize "${doom-hierarchy:120} " 'face 'org-roam-title)
                                            ;; (propertize "${backlinkscount:5} " 'face 'org-formula)
                                            (propertize "${tags:60}" 'face 'org-tag))
            org-roam-node-annotation-function
            (lambda (node) (marginalia--time (org-roam-node-file-mtime node))))

      (defun org-roam-create-id-sync-db ()
        "Rebuild the `org-mode' and `org-roam' cache."
        (interactive)
        (org-id-get-create)
        ;; (org-id-update-id-locations)
        (org-roam-db-sync)
        (org-roam-update-org-id-locations))

    ;;;;; org-roam-headline

      (unless *is-termux*
        ;; (load-file (concat dotspacemacs-directory "layers/jh-org/org-roam-config.el"))
        ;; (load-file (concat dotspacemacs-directory "layers/jh-org/org-roam-headline.el"))
        ;; (setq org-roam-db-location (concat org-directory "org-roam.db"))
        ;; (setq org-roam-headline-db-location (concat org-directory "org-roam-headline.db"))
        ;; (run-with-idle-timer 3 t #'eli/update-org-roam-db) ; manually on
        (org-roam-db-autosync-enable)
        )
      ) ;; end-of org-roam

    ;;;;; DONT citar-org-roam
    ;; (defun jh-org/init-citar-org-roam ()
    ;;   (use-package citar-org-roam :after citar))

    ;;;;; DONT consult-org-roam

    ;; (defun jh-org/init-consult-org-roam ()
    ;;   (use-package consult-org-roam
    ;;     :after org-roam consult
    ;;     :custom
    ;;     ;; Configure a custom narrow key for `consult-buffer'
    ;;     (consult-org-roam-buffer-narrow-key ?k) ; Knowledge
    ;;     ;; Display org-roam buffers right after non-org-roam buffers
    ;;     ;; in consult-buffer (and not down at the bottom)
    ;;     (consult-org-roam-buffer-after-buffers nil)
    ;;     :config
    ;;     (setq consult-org-roam-grep-func #'consult-ripgrep)
    ;;     ;; Eventually suppress previewing for certain functions
    ;;     (consult-customize
    ;;      consult-org-roam-file-find
    ;;      consult-org-roam-search
    ;;      consult-org-roam-backlinks
    ;;      consult-org-roam-forward-links
    ;;      :preview-key '("M-." "C-SPC"
    ;;                     ;; :debounce 0.3 "C-M-j" "C-M-k" ; conflict puni
    ;;                     :debounce 0.3 "<up>" "<down>" "C-n" "C-p"
    ;;                     ))
    ;;     ;; Activate the minor-mode
    ;;     (consult-org-roam-mode 1)))

    ;;;; side-notes

    (defun jh-org/init-side-notes ()
      (use-package side-notes
        :init
        (add-hook 'side-notes-hook #'visual-line-mode) ; Good
        ))

    ;;;; org-rich-yank

    ;; Do you often yank source code into your org files, manually surrounding it in
    ;; #+BEGIN_SRC blocks? This package will give you a new way of pasting that
    ;; automatically surrounds the snippet in blocks, marked with the major mode of
    ;; where the code came from, and adds a link to the source file after the block.

    ;; 소스 코드를 조직 파일로 가져와서 #+BEGIN_SRC 블록으로 수동으로 둘러싸는
    ;; 경우가 자주 있나요? 이 패키지는 코드가 어디에서 왔는지 주요 모드로 표시된
    ;; 블록으로 스니펫을 자동으로 둘러싸고 블록 뒤에 소스 파일에 대한 링크를
    ;; 추가하는 새로운 붙여넣기 방법을 제공합니다.

    (defun jh-org/post-init-org-rich-yank ()
      ;; https://github.com/unhammer/org-rich-yank If you want to change how the
      ;; source block or link is formatted, you can do so by setting
      ;; org-rich-yank-format-paste to a function. For example, links to local files
      ;; might be useful in your org document but not so useful in exported content,
      ;; so you may want to make such a link a comment line.

      ;; 소스 블록 또는 링크의 서식을 변경하려면 org-rich-yank-format-paste 을 함수로
      ;; 설정하여 변경할 수 있습니다. 예를 들어 로컬 파일에 대한 링크는 조직
      ;; 문서에서는 유용하지만 내보낸 콘텐츠에서는 유용하지 않을 수 있으므로 이러한
      ;; 링크를 주석 줄로 만들 수 있습니다.
      (defun my/org-rich-yank-format-paste (language contents link)
        "Based on `org-rich-yank--format-paste-default'."
        (format "#+BEGIN_SRC %s\n%s\n#+END_SRC\n#+comment: %s"
                language
                (org-rich-yank--trim-nl contents)
                link))
      (setq org-rich-yank-format-paste #'my/org-rich-yank-format-paste)
      )

    ;;;; yankpad

    ;; [[https://github.com/Kungsgeten/yankpad][yankpad]] is an add-on that enables easy management of yasnippet
    ;; snippets within an Org-mode file. I do define Org-mode-independent
    ;; snippets with the basic yasnippet methods. Any snippet that is used
    ;; within Org-mode only is defined in my yankpad file.

    (defun jh-org/init-yankpad ()
      (use-package yankpad
        :defer 6
        :after org
        :init
        (bind-keys :prefix-map yank-map
                   :prefix "C-c Y"
                   ("c" . yankpad-set-category)
                   ("e" . yankpad-edit)
                   ("i" . yankpad-insert)
                   ("m" . yankpad-map)
                   ("r" . yankpad-reload)
                   ("x" . yankpad-expand))
        :config
        (setq yankpad-file (concat org-directory "templates/yankpad.org"))
        ;; If you want to complete snippets using company-mode
        ;; (add-to-list 'company-backends #'company-yankpad)
        ;; ;; If you want to expand snippets with hippie-expand
        ;; (add-to-list 'hippie-expand-try-functions-list #'yankpad-expand)
        )
      )

    ;;;; org-glossary

    (defun jh-org/init-org-glossary ()
      (use-package org-glossary
        :after org
        :defer 4
        :config
        (setq org-glossary-collection-root (concat org-roam-directory "notes/"))
        ;; (setq org-glossary-global-terms nil)
        (add-hook 'org-mode-hook 'org-glossary-mode)
        ;; (setq org-glossary-automatic nil) ;; disable auto-export
        ))

    ;; sample from tecosaur/org-glossary
    ;; (defun +org-glossary--latex-cdef (backend info term-entry form &optional ref-index plural-p capitalized-p extra-parameters)
    ;;   (org-glossary--export-template
    ;;    (if (plist-get term-entry :uses)
    ;;        "*%d*\\emsp{}%v\\ensp{}@@latex:\\labelcpageref{@@%b@@latex:}@@\n"
    ;;      "*%d*\\emsp{}%v\n")
    ;;    backend info term-entry ref-index
    ;;    plural-p capitalized-p extra-parameters))
    ;; (org-glossary-set-export-spec
    ;;  'latex t
    ;;  :backref "gls-%K-use-%r"
    ;;  :backref-seperator ","
    ;;  :definition-structure #'+org-glossary--latex-cdef)

    ;;;; Spaced-Repetition
    ;;;;; org-drill

    (defun jh-org/init-org-drill ()
      (use-package org-drill :after org :defer 10)
      )

    ;;;;; TODO org-fc

    (defun jh-org/init-org-fc ()
      (use-package org-fc
        :after org hydra
        :commands org-fc-hydra/body
        :defer 5
        :config
        (require 'org-fc-hydra)
        (require 'org-fc-keymap-hint)
        (setq org-fc-directories (concat org-directory "fc/"))

        ;; https://www.leonrische.me/fc/use_with_evil-mode.html
        (evil-define-minor-mode-key '(normal insert emacs) 'org-fc-review-flip-mode
          (kbd "RET") 'org-fc-review-flip
          (kbd "n") 'org-fc-review-flip
          (kbd "s") 'org-fc-review-suspend-card
          (kbd "q") 'org-fc-review-quit)

        (evil-define-minor-mode-key '(normal insert emacs) 'org-fc-review-rate-mode
          (kbd "a") 'org-fc-review-rate-again
          (kbd "h") 'org-fc-review-rate-hard
          (kbd "g") 'org-fc-review-rate-good
          (kbd "e") 'org-fc-review-rate-easy
          (kbd "s") 'org-fc-review-suspend-card
          (kbd "q") 'org-fc-review-quit)

        ;; (add-to-list 'org-fc-custom-contexts
        ;;              '(french-cards . (:filter (tag "french"))))
        )
      )

    ;;;; org-ql

    (defun jh-org/init-org-ql ()
      (use-package org-ql :defer 10))

    ;;;; consult-notes

    (defun jh-org/init-consult-notes ()
      (use-package consult-notes
        :demand t
        :commands (consult-notes consult-notes-search-in-all-notes)))

    ;;;; org-randomnote

    ;; https://github.com/mwfogleman/org-randomnote
    (defun jh-org/init-org-randomnote ()
      (use-package org-randomnote
        :after org
        :if (not (or my/remote-server *is-termux*))
        :defer 10
        :config
        (setq org-randomnote-candidates
              (find-lisp-find-files "~/sync/org/roam/oldnotes" "\.org$"))
        ;; (setq org-randomnote-candidates '("~/Notes/Schedule.org" "~/Notes/Incoming.org" "~/Notes/Archive.org"))
        )
      )

    ;;;; org-tidy

    (defun jh-org/init-org-tidy ()
      (use-package org-tidy :ensure t
        ;; :config
        ;; (add-hook 'org-mode-hook #'org-tidy-mode)
        )
      )

    ;;;; Denote

    ;; /home/junghan/sync/man/dotsamples/vanilla/writing-dotfiles-pprevos/init.el

    ;; (use-package org
    ;;   :bind
    ;;   (("C-c c" . org-capture)
    ;;    ("C-c l" . org-store-link))
    ;;   :custom
    ;;   ;; Set default file for fleeting notes
    ;;   (org-default-notes-file
    ;;    (car (denote-directory-files-matching-regexp "inbox")))
    ;;   ;; Capture templates
    ;;   (org-capture-templates
    ;;    '(("f" "Fleeting note" item
    ;;       (file+headline org-default-notes-file "Notes")
    ;;       "- %?")
    ;;      ("t" "New task" entry
    ;;       (file+headline org-default-notes-file "Tasks")
    ;;       "* TODO %i%?"))))

    ;;;;; TMR May Ring (tmr is used to set timers)

    ;; [[https://takeonrules.com/2023/02/25/my-lesser-sung-packages-of-emacs/][My Lesser Sung Packages of Emacs // Take on Rules]]

    ;; Read the manual: <https://protesilaos.com/emacs/tmr>.
    (defun jh-org/init-tmr ()
      (use-package tmr
        :after embark
        :config
        (setq tmr-sound-file "/usr/share/sounds/freedesktop/stereo/alarm-clock-elapsed.oga"
              tmr-notification-urgency 'normal
              tmr-description-list 'tmr-description-history)

        (defvar tmr-action-map
          (let ((map (make-sparse-keymap)))
            (define-key map "k" #'tmr-remove)
            (define-key map "r" #'tmr-remove)
            (define-key map "R" #'tmr-remove-finished)
            (define-key map "c" #'tmr-clone)
            (define-key map "e" #'tmr-edit-description)
            (define-key map "s" #'tmr-reschedule)
            map))
        ;; (define-key global-map (kbd "M-g M-t") 'tmr-action-map)

        (with-eval-after-load 'embark
          (add-to-list 'embark-keymap-alist '(tmr-timer . tmr-action-map))
          (cl-loop
           for cmd the key-bindings of tmr-action-map
           if (commandp cmd) do
           (add-to-list 'embark-post-action-hooks (list cmd 'embark--restart))))

        )
      )

    ;;;;; Denote

    (defun jh-org/init-denote ()
      (use-package denote
        :ensure t
        :init
        (require 'denote-org-dblock)
        ;; :custom-face
        ;; (denote-faces-link ((t (:weight bold :slant italic))))
        :config
        ;; (setq denote-directory (concat org-directory "denote/"))
        (setq denote-directory org-notes-directory)
        ;; (setq denote-directory (expand-file-name org-notes-directory)) ;; too long
        (setq denote-sort-components '(signature title keywords identifier))
        (setq denote-known-keywords '("emacs" "philosophy" "politics" "economics"))
        (setq denote-infer-keywords t)
        ;; (setq denote-sort-keywords t)

        ;; By default, we do not show the context of links.  We just display
        ;; file names.  This provides a more informative view.
        (setq denote-backlinks-show-context t)

        ;; Pick dates, where relevant, with Org's advanced interface:
        (setq denote-date-prompt-use-org-read-date nil)

        ;; If you use Markdown or plain text files (Org renders links as buttons
        ;; right away)
        ;; (add-hook 'find-file-hook #'denote-link-buttonize-buffer)

        ;; We use different ways to specify a path for demo purposes.
        ;; (setq denote-dired-directories
        ;;       (list denote-directory            ; The Zettelkasten directory
        ;;             ;; (thread-last denote-directory (expand-file-name "excerpts"))
        ;;             (thread-last denote-directory "excerpts")
        ;;             ;; (thread-last denote-directory (expand-file-name "attachments"))
        ;;             ;; (expand-file-name "~/Documents/books")
        ;;             ))

        (setq denote-dired-directories
              (list denote-directory
                    (concat denote-directory "excerpts/")))

        (add-hook 'dired-mode-hook #'denote-dired-mode)

        ;; OR if only want it in `denote-dired-directories':
        ;; (add-hook 'dired-mode-hook #'denote-dired-mode-in-directories)

        ;; Automatically rename Denote buffers using the `denote-rename-buffer-format'.
        (denote-rename-buffer-mode 1)

        ;; Denote DOES NOT define any key bindings.  This is for the user to
        ;; decide.  For example:
        (define-prefix-command 'denote-map)
        (define-key global-map (kbd "C-c w") 'denote-map)
        (let ((map denote-map))
          ;; (define-key map (kbd "n") #'denote)
          (define-key map (kbd "t") #'denote-type)
          (define-key map (kbd "T") #'denote-template)
          (define-key map (kbd "D") #'denote-date)
          (define-key map (kbd "z") #'denote-signature) ; "zettelkasten" mnemonic
          (define-key map (kbd "s") #'denote-subdirectory)
          ;; If you intend to use Denote with a variety of file types, it is
          ;; easier to bind the link-related commands to the `global-map', as
          ;; shown here.  Otherwise follow the same pattern for `org-mode-map',
          ;; `markdown-mode-map', and/or `text-mode-map'.
          (define-key map (kbd "l") #'denote-link) ; "insert" mnemonic
          (define-key map (kbd "L") #'denote-add-links)
          (define-key map (kbd "b") #'denote-backlinks)
          (define-key map (kbd "f f") #'denote-find-link)
          (define-key map (kbd "f b") #'denote-find-backlink)
          ;; Note that `denote-rename-file' can work from any context, not just
          ;; Dired bufffers.  That is why we bind it here to the `global-map'.
          (define-key map (kbd "r") #'denote-region) ; "contents" mnemonic
          (define-key map (kbd "R") #'denote-rename-file-using-front-matter)
          (define-key map (kbd "M-r") #'denote-rename-file)

          (define-key map (kbd "k") #'denote-keywords-add)
          (define-key map (kbd "K") #'denote-keywords-remove)

          (define-key map (kbd "i") #'denote-org-dblock-insert-links)
          (define-key map (kbd "I") #'denote-org-dblock-insert-backlinks)
          )

        ;; Key bindings specifically for Dired.
        (let ((map dired-mode-map))
          (define-key map (kbd "C-c C-d C-i") #'denote-link-dired-marked-notes)
          (define-key map (kbd "C-c C-d C-r") #'denote-dired-rename-files)
          (define-key map (kbd "C-c C-d C-k") #'denote-dired-rename-marked-files-with-keywords)
          (define-key map (kbd "C-c C-d C-R") #'denote-dired-rename-marked-files-using-front-matter))

        (with-eval-after-load 'org-capture
          (setq denote-org-capture-specifiers "%l\n%i\n%?")
          (add-to-list 'org-capture-templates
                       '("d" "denote create(with denote.el)" plain
                         (file denote-last-path)
                         #'denote-org-capture
                         :no-save t
                         :immediate-finish nil
                         :kill-buffer t
                         :jump-to-captured t)))

        ;; Also check the commands `denote-link-after-creating',
        ;; `denote-link-or-create'.  You may want to bind them to keys as well.

        ;; If you want to have Denote commands available via a right click
        ;; context menu, use the following and then enable
        ;; `context-menu-mode'.
        ;; (add-hook 'context-menu-functions #'denote-context-menu)

        (with-eval-after-load 'consult-notes
          (setq consult-notes-file-dir-sources '(
                                                 ;; ("Workflow"  ?w (expand-file-name org-workflow-directory))
                                                 ;; ("Zettels"   ?z ,org-roam-directory)
                                                 ;; ("Excerpts"  ?e "~/sync/org/denote/excerpts/")
                                                 ("Clone-notes"  ?c  "~/nosync/clone-notes/")
                                                 ))
          ;; Set org-roam integration, denote integration, or org-heading integration e.g.:
          (setq consult-notes-org-headings-files
                '("~/sync/org/roam/workflow/20230303T030300--contacts__agenda.org"
                  "~/sync/org/roam/workflow/20230202T020200--inbox__refile.org"
                  "~/sync/org/roam/workflow/20230219T035500--links__agenda.org"
                  "~/sync/org/roam/workflow/20230101T010100--project__agenda.org"
                  "~/sync/org/roam/workflow/quote.org"
                  "~/sync/org/roam/notes/20231005T133900--filetags__index_terms.org"
                  "~/sync/org/elfeed/elfeed.org"
                  ))

          (consult-notes-org-headings-mode)

          (setq consult-notes-denote-display-id nil)
          (consult-notes-denote-mode)

          ;; search only for text files in denote dir
          ;; (setq consult-notes-denote-files-function (function denote-directory-text-only-files))

          ;; (defun consult-notes-my-embark-function (cand)
          ;;   "Do something with CAND"
          ;;   (interactive "fNote: ")
          ;;   (my-function))
          ;; (defvar-keymap consult-notes-map
          ;;   :doc "Keymap for Embark notes actions."
          ;;   :parent embark-file-map
          ;;   "m" #'consult-notes-my-embark-function)
          ;; (add-to-list 'embark-keymap-alist `(,consult-notes-category . consult-notes-map))
          ;; ;; make embark-export use dired for notes
          ;; (setf (alist-get consult-notes-category embark-exporters-alist) #'embark-export-dired)
          ) ;; end consult-notes

        (with-eval-after-load 'citar-denote
          (citar-denote-mode t))

        (progn
          ;; Or write a small function that you can then modify without
          ;; revaluating the hook:
          ;; (defun my-denote-tmr ()
          ;;   (tmr "10" "Practice writing in my journal"))
          ;; (add-hook 'denote-journal-extras-hook 'my-denote-tmr)

          ;; Or to make it fully featured, define variables for the duration and the
          ;; description and set it up so that you only need to modify those:
          (defvar my-denote-tmr-duration "10")
          (defvar my-denote-tmr-description "Practice writing in my journal")
          (defun my-denote-tmr ()
            (tmr my-denote-tmr-duration my-denote-tmr-description))
          (add-hook 'denote-journal-extras-hook 'my-denote-tmr)
          ) ; end progn
        )
      )

    ;;;;; citar-denote

    (defun jh-org/init-citar-denote ()
      (use-package citar-denote
        :ensure t
        :custom
        ;; Use package defaults
        (citar-open-always-create-notes nil)
        (citar-denote-file-type 'org)
        (citar-denote-subdir nil)
        (citar-denote-keyword "bib")
        (citar-denote-use-bib-keywords nil)
        (citar-denote-title-format "title")
        (citar-denote-title-format-authors 1)
        (citar-denote-title-format-andstr "and")
        )
      )

    ;; 읽어볼 것 https://github.com/pprevos/denote-explore
    ;; (defun jh-org/init-denote-explore ()
    ;;  (use-package denote-explore :after denote :defer 5))

    ;;;;; DONT ZK

    ;; (defun gr/zk-new-note-header (title new-id &optional orig-id)
    ;;   "Insert header in new notes with args TITLE and NEW-ID.
    ;; Optionally use ORIG-ID for backlink."
    ;;   (insert (format "#+title: %s\n#+subtitle:\n#+date: %s\n#+filetags: :zk:\n#+identifier: %s\n#+description:\n\n===\n#+tags: \n" title (format-time-string "[%Y-%m-%d %a %H:%M]") new-id))

    ;;   (when (ignore-errors (zk--parse-id 'title orig-id)) ;; check for file
    ;;     (progn
    ;;       (insert "===\n<- ")
    ;;       (zk--insert-link-and-title orig-id (zk--parse-id 'title orig-id))
    ;;       (newline)))
    ;;   (insert "===\n\n\n"))

    ;; (defun gr/zk-insert-tag (tag)
    ;;   (interactive)
    ;;   (unless current-prefix-arg
    ;;     (goto-char (point-min))
    ;;     (when (re-search-forward "#\\+tags:" nil t)
    ;;       (goto-char (match-beginning 0))
    ;;       (end-of-line)
    ;;       (insert " ")))
    ;;   (insert tag))

    ;; (defun zk-org-try-to-follow-link (fn &optional arg)
    ;;   "When `org-open-at-point' FN fails, try `zk-follow-link-atpoint'.
    ;; Optional ARG."
    ;;   (let ((org-link-search-must-match-exact-headline t))
    ;;     (condition-case nil
    ;; 	    (apply fn arg)
    ;;       (error (unless (ignore-errors (zk-follow-link-at-point))
    ;;                (message "Invalid org-link type"))))))
    ;; (advice-add 'org-open-at-point :around #'zk-org-try-to-follow-link)

    ;; ;; redefine own function
    ;; (defun zk--grep-tag-list ()
    ;;   "Return list of tags from all notes in zk directory."
    ;;   (delete-dups
    ;;     (split-string
    ;;       (string-join
    ;;         (split-string
    ;;           (shell-command-to-string (concat
    ;;                                      "grep -ohir --include \\*."
    ;;                                      zk-file-extension
    ;;                                      " -e "
    ;;                                      (shell-quote-argument
    ;;                                        "+tags:.*")
    ;;                                      (shell-quote-argument
    ;;                                        zk-tag-regexp)
    ;;                                      " "
    ;;                                      zk-directory " 2>/dev/null"))
    ;;           "\\+tags:" "\s" "\n"))
    ;;       " "))
    ;;   )

    ;; (defun jh-org/init-zk ()
    ;;   (use-package zk
    ;;     :defer 1
    ;;     :commands (zk-org-try-to-follow-link)
    ;;     ;; :hook ; 후크는 따로 뺐다.
    ;;     ;; (completion-at-point-functions . zk-completion-at-point)
    ;;     ;; (completion-at-point-functions . gr/mmd-citation-completion-at-point)
    ;;     :custom
    ;;     (zk-file-extension "org")
    ;;     (zk-tag-regexp "\\s#[a-zA-Z0-9]\\+") ; default

    ;;     (zk-new-note-header-function #'gr/zk-new-note-header)
    ;;     (zk-tag-insert-function 'gr/zk-insert-tag)

    ;;     (zk-link-and-title 'ask)
    ;;     (zk-new-note-link-insert 'ask)

    ;;     ;; Consult
    ;;     ;; (zk-search-function #'zk-xref) ;; #'zk-consult-grep) ;; #'zk-grep ;;
    ;;     ;; (zk-search-function 'zk-consult-grep)
    ;;     (zk-current-notes-function nil)

    ;;     ;; Denote Integration
    ;;     (zk-id-time-string-format "%Y%m%dT%H%M%S")
    ;;     (zk-id-regexp "\\([0-9]\\{8\\}\\)\\(T[0-9]\\{6\\}\\)")
    ;;     (zk-file-name-separator "-")

    ;;     :config
    ;;     (zk-setup-auto-link-buttons)
    ;;     ;; (zk-setup-embark)

    ;;     (setq zk-directory org-notes-directory) ; "~/sync/org/roam/notes/"

    ;;     (setq zk-link-format "[[%s]]")
    ;;     (setq zk-link-and-title-format "%t [[%i]]")
    ;;     (setq zk-completion-at-point-format "%t [[%i]]")

    ;;     ;; Denote 스타일을 사용한다.
    ;;     ;; (setq zk-link-format "[[denote:%s]]")
    ;;     ;; (setq zk-link-and-title-format "%t [[denote:%i]]")
    ;;     ;; (setq zk-completion-at-point-format "%t [[denote:%i]]")

    ;;     (setq zk-tag-search-function #'zk-consult-grep-tag-search)
    ;;     )
    ;;   )

    ;;;; ox-hugo

    (defun jh-org/init-ox-hugo ()
      (use-package ox-hugo
        :after ox
        :defer 3
        )
      )

    ;;;; DONT org-superstar

    ;; (defun jh-org/post-init-org-superstar ()
    ;;   (setq org-superstar-leading-bullet ?\s)
    ;;   (setq org-superstar-item-bullet-alist
    ;;         '((?* . ?‣) ; ?⋆
    ;;           (?+ . ?➤) ;; ?➤ ?•
    ;;           (?- . ?◦)))
    ;;   (setq org-superstar-remove-leading-stars nil)

    ;;   ;; ☯
    ;;   ;; (setq org-superstar-headline-bullets-list '("☀" "☀" "☀" "☀" "☀" "☀")) ; black sun with rays
    ;;   ;; (setq org-superstar-headline-bullets-list  '("♈" "♉" "♊" "♌" "♍" "♏" "♓" "♎"))
    ;;   (setq org-superstar-headline-bullets-list nil)
    ;;   )

    ;;;; DONT org-super-agenda

    ;; (defun jh-org/init-org-super-agenda ()
    ;;   (use-package org-super-agenda
    ;;     :after org
    ;;     :init
    ;;     (autoload 'org-super-agenda "org-agenda")
    ;;     )
    ;;   )

    ;;; packages.el ends here

    ```


#### <span class="section-num">3.10.3</span> The `jh-org` funcs.el {#h:e56cc8e9-a30a-4f11-b36e-8ae0f9a7d01c}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;; ~/sync/org/roam/configs/jh-org.org

;;; Functions

;;;; math-preview

;;;###autoloads
(defun auto/math-preview-all ()
  "Auto update clock table."
  (interactive)
  (when (derived-mode-p 'org-mode)
    (save-excursion
      (goto-char 0)
      (unless (string-equal (cadar (org-collect-keywords '("NO_MATH_PREVIEW"))) "t")
        (when (re-search-forward "\\$\\|\\\\[([]\\|^[ \t]*\\\\begin{[A-Za-z0-9*]+}" (point-max) t)
          (math-preview-all))))))

;;;; org-capture and org-id
;;;;; my/org-capture-goto-link

;; /home/junghan/.emacs.tshu/lisp/lang-org.el
(defun org-capture-goto-link ()
  (interfative)
  (let ((file (nth 1 (org-capture-get :target)))
        (headline (plist-get org-store-link-plist :description))
        (link (plist-get org-store-link-plist :link)))
    (org-capture-put :target (list 'file+headline file headline))
    (widen)
    (goto-char (point-min))
    (let (case-fold-search)
      (if (re-search-forward
           (format org-complex-heading-regexp-format
                   (regexp-quote headline)) nil t)
          (org-end-of-subtree)
        (org-capture-put :flag t)
        (goto-char (point-max))
        (or (bolp) (insert "\n"))
        (insert "* TODO " headline "\n")
        (insert "[[" link "]]\n")
        (point)))))

;;;;; my/get-id-to-clipboard

(defun my/get-id-to-clipboard() "Copy an ID link with the
  headline to killring, if no ID is there then create a new unique
  ID.  This function works only in org-mode or org-agenda buffers.
  The purpose of this function is to easily construct id:-links to
  org-mode items. If its assigned to a key it saves you marking the
  text and copying to the killring."
       (interactive)
       (when (eq major-mode 'org-agenda-mode) ;switch to orgmode
         (org-agenda-show)
         (org-agenda-goto))
       (when (eq major-mode 'org-mode) ; do this only in org-mode buffers
         (setq mytmphead (nth 4 (org-heading-components)))
         (setq mytmpid (funcall 'org-id-get-create))
         (setq mytmplink (format "[[id:%s][%s]]" mytmpid mytmphead))
         (kill-new mytmplink)
         (message "Copied %s to killring (clipboard)" mytmplink)
         ))
(let ((map global-map))
  (define-key map (kbd "C-c j g") 'my/get-id-to-clipboard)
  )

;;;;; DONT my/org-note-get-id-create

;; /fuco1-dotfiles/files/org-defs.el:1312
;; (defvar my/org-node-is-note-capture nil)
;; (defvar my/org-node-last-id nil)

;; (defun my/org-note-get-id-create ()
;;   (save-excursion
;;     (org-back-to-heading t)
;;     (when (search-forward "<note-placeholder>" (line-end-position) t)
;;       (let ((id (org-id-get-create)))
;;         (org-back-to-heading t)
;;         (search-forward "<note-placeholder>" (line-end-position) t)
;;         (replace-match id)
;;         (setq my/org-node-last-id id)))))

;; (defun my/org-note-create-link-to-note ()
;;   (save-excursion
;;     (when (and my/org-node-is-note-capture
;;                (not org-note-abort))
;;       (-let* (((beg . end) my/org-node-is-note-capture)
;;               (text (delete-and-extract-region beg end)))
;;         (goto-char beg)
;;         (insert (format "[[id:%s][%s]]" my/org-node-last-id text)))))
;;   (setq my/org-node-is-note-capture nil)
;;   )

;; (add-hook 'org-capture-mode-hook 'my/org-note-get-id-create)
;; (add-hook 'org-capture-after-finalize-hook 'my/org-note-create-link-to-note)

;;;; split and indirec orgtree

;; copy from writers-dot-spacemaccs
(defun my/split-and-indirect-orgtree ()
  "Splits window to the right and opens an org tree section in it"
  (interactive)
  (split-window-right)
  (org-tree-to-indirect-buffer)
  (windmove-right))

(defun my/kill-and-unsplit-orgtree ()
  "Kills the cloned buffer and deletes the window."
  (interactive)
  (kill-this-buffer)
  (delete-window))

;;;; org-toggle-emphasis-markers

(defun my/org-toggle-emphasis-markers ()
  (interactive)
  (setq org-hide-emphasis-markers (not org-hide-emphasis-markers))
  (font-lock-fontify-buffer :interactively))

;;;; open org files

(defun open-org-refile-file ()
  "Open `org-refile-file'"
  (interactive)
  (find-file org-refile-file))

(defalias 'open-org-default-notes-file #'open-org-refile-file
  "Open `org-default-notes-file'")

(defun org-notes-files ()
  "Get the list of `org-mode' file in `org-note-directory'."
  (interactive)
  (find-lisp-find-files org-notes-directory "\.org$"))

(defun org-workflow-files ()
  "Get the list of `org-mode' file in `org-note-directory'."
  (interactive)
  (find-lisp-find-files org-workflow-directory "\.org$"))

;;;; align-comments

(defun my/align-comments (beginning end)
  "Align comments in region."
  (interactive "*r")
  (align-regexp beginning end (concat "\\(\\s-*\\)"
                                      (regexp-quote comment-start)) nil 2))

;;;; indent-buffer

(defun my/indent-buffer ()
  "Indent buffer."
  (interactive)
  (indent-region (point-min) (point-max)))

;;;; comment-or-uncomment-region

(defun my/comment-or-uncomment-region ()
  "Comment or uncomment region with just a character (e.g. '/'). If a region is
active call comment-or-uncomment-region, otherwise just insert the given char."
  (interactive)
  (call-interactively (if (region-active-p)
                          'comment-or-uncomment-region
                        'self-insert-command)))

;;;; org-indent-src-block

(defun my/org-modern-indent-src-block ()
  (interactive)
  (org-edit-special)
  (my/indent-buffer)
  (org-edit-src-exit))

;;;; org-sort-by-priority

(defun my/org-modern-sort-by-priority ()
  "Sort entries in level=2 by priority."
  (interactive)
  (org-map-entries (lambda () (condition-case nil
                                  (org-sort-entries nil ?p)
                                (error nil)))
                   "LEVEL=1")
  (org-set-startup-visibility))

;;;; org-delete-link

(defun org-delete-link ()
  "Remove the link part of an org-mode link at point and keep
  only the description"
  (interactive)
  (let ((elem (org-element-context)))
    (when (eq (car elem) 'link)
      (let* ((content-begin (org-element-property :contents-begin elem))
             (content-end  (org-element-property :contents-end elem))
             (link-begin (org-element-property :begin elem))
             (link-end (org-element-property :end elem)))
        (when (and content-begin content-end)
          (let ((content (buffer-substring-no-properties content-begin content-end)))
            (delete-region link-begin link-end)
            (insert (concat content " "))))))))

;;;; org-insert-file-link

(defun org-insert-file-link ()
  "Insert a file link.  At the prompt, enter the filename."
  (interactive)
  (insert (format "[[%s]]" (org-link-complete-file))))

;;;; org-insert-link-dwim

(defun org-insert-link-dwim ()
  "Like `org-insert-link' but with personal dwim preferences."
  (interactive)
  (let* ((point-in-link (org-in-regexp org-link-any-re 1))
         (clipboard-url (when (string-match-p "^http" (current-kill 0))
                          (current-kill 0)))
         (region-content (when (region-active-p)
                           (buffer-substring-no-properties (region-beginning)
                                                           (region-end)))))
    (cond ((and region-content clipboard-url (not point-in-link))
           (delete-region (region-beginning) (region-end))
           (insert (org-make-link-string clipboard-url region-content)))
          ((and clipboard-url (not point-in-link))
           (insert (org-make-link-string
                    clipboard-url
                    (read-string "Title: "
                                 (with-current-buffer (url-retrieve-synchronously clipboard-url)
                                   (dom-text (car
                                              (dom-by-tag (libxml-parse-html-region
                                                           (point-min)
                                                           (point-max))
                                                          'title))))))))
          (t
           (call-interactively 'org-insert-link)))))

;;;; efs/presentation setup / end

(defun efs/presentation-setup ()
  (hide-mode-line-mode 1)

  ;; Display images inline
  (org-display-inline-images) ;; Can also use org-startup-with-inline-images

  ;; (spacemacs/toggle-indent-guide-off)
  (spacemacs/toggle-fill-column-indicator-off)
  (spacemacs/toggle-line-numbers-off)

  ;; A) Scale the text.  The next line is for basic scaling:
  (setq text-scale-mode-amount 2)
  (text-scale-mode 1)

  ;; B) This option is more advanced, allows you to scale other faces too
  ;; (setq-local face-remapping-alist '((default (:height 1.5) variable-pitch) ; variable-pitch
  ;;                                    (header-line (:height 2.0) variable-pitch) ; variable-pitch
  ;;                                    (org-document-title (:height 1.5) org-document-title)
  ;;                                    (org-code (:height 1.55) org-code)
  ;;                                    (org-verbatim (:height 1.55) org-verbatim)
  ;;                                    (org-block (:height 1.25) org-block)
  ;;                                    (org-block-begin-line (:height 0.7) org-block)))
  )

(defun efs/presentation-end ()
  (hide-mode-line-mode 0)

  ;; (spacemacs/toggle-indent-guide-on)
  (spacemacs/toggle-fill-column-indicator-on)
  (spacemacs/toggle-line-numbers-on)

  ;; A) Turn off text scale mode (or use the next line if you didn't use text-scale-mode)
  (text-scale-mode 0)

  ;; B) If you use face-remapping-alist, this clears the scaling:
  ;; (setq-local face-remapping-alist '((default fixed-pitch default)))
  )

;;;; org-show-level

(defun org-show-level-1 ()
  (interactive)
  (org-content 1))

(defun org-show-level-2 ()
  (interactive)
  (org-content 2))

(defun org-show-level-3 ()
  (interactive)
  (org-content 3))

(defun org-show-level-4 ()
  (interactive)
  (org-content 4))

;;;; show-duplicate-lines

(defun show-duplicate-lines ()
  "Display all duplicate lines in the current buffer."
  (interactive)
  (let ((lines (split-string (buffer-string) "\n" t))
        (seen-lines '())
        (dup-lines '()))
    (dolist (line lines)
      (if (member line seen-lines)
          (setq dup-lines (cons line dup-lines))
        (setq seen-lines (cons line seen-lines))))
    (if dup-lines
        (with-output-to-temp-buffer "*Duplicate Lines*"
          (dolist (line (reverse dup-lines))
            (princ (concat line "\n"))))
      (message "No duplicate lines found."))))

;;;; get/insert heading-title / category

;; (defun cc-todo-item () ; from choi
;;   (interactive)
;;   (org-insert-heading)
;;   (org-schedule nil)
;;   (org-shiftup)
;;   (org-shiftright)
;;   (end-of-line))

;; http://article.gmane.org/gmane.emacs.orgmode/10256
(defun my/org-get-heading-title ()
  "Returns the heading of the current entry as a string, without the leading stars, the TODO keyword or the tags."
  (let ((title-with-props (org-get-heading t))
        (keyword (org-get-todo-state)))
    (substring-no-properties title-with-props (if keyword (1+ (length keyword))))))

(defun my/org-insert-heading-category ()
  "Insert a :CATEGORY: property and it's value to the PROPERTY drawer at point."
  (interactive)
  (let ((point (point)))
    (org-entry-put point "CATEGORY" (org-get-heading-title))))

(defun my/org-insert-heading-categories-all ()
  "Insert :CATEGORY: properties to each headlines indented level 2."
  (interactive)
  (org-map-entries
   (lambda ()
     (if (eq 2 (org-current-level))
         (org-insert-heading-category)))))

(defun my/insert-header-from-note-name ()
  (interactive)
  (let ((date (encode-time (org-parse-time-string (substring (buffer-name) 5 -4)))))
    (save-excursion
      (goto-char (point-min))
      (insert (concat "#+TITLE: " (org-format-time-string "%Y-%m-%d " date nil)  "\n"
                      "#+date: " (format-time-string "[%Y-%m-%d]" date) "\n\n")
              )))
  )

;;;; org-narrow-to-item

;; https://emacs-china.org/t/org-mode-narrow-to-sublist/24682/5
(defun my/org-narrow-to-item ()
  "Narrow buffer to the current item.
Throw an error when not in a list."
  (interactive)
  (save-excursion
    (narrow-to-region
     (progn (org-beginning-of-item) (point))
     (progn (org-end-of-item) (1- (point))))))

;;;; my/view-text-file-as-info-manual

;; View ‘info’, ‘texi’, ‘org’ and ‘md’ files as ‘Info’ manual
(defun my/view-text-file-as-info-manual ()
  (interactive)
  (require 'ox-texinfo)
  (let ((org-export-with-broken-links 'mark))
    (pcase (file-name-extension (buffer-file-name))
      (`"info"
       (info (buffer-file-name)))
      (`"texi"
       (info (org-texinfo-compile (buffer-file-name))))
      (`"org"
       (info (org-texinfo-export-to-info)))
      (`"md"
       (let ((org-file-name (concat (file-name-sans-extension (buffer-file-name)) ".org")))
         (apply #'call-process "pandoc" nil standard-output nil
                `("-f" "markdown"
                  "-t" "org"
                  "-o" , org-file-name
                  , (buffer-file-name)))
         (with-current-buffer (find-file-noselect org-file-name)
           (info (org-texinfo-export-to-info)))))
      (_ (user-error "Don't know how to convert `%s' to an `info' file"
                     (file-name-extension (buffer-file-name)))))))

;;;; my/org-outdent-or-promote

(defun my/org-outdent-or-promote ()
  "Run either org-outdent-item-tree or org-promote-subtree,
depending on which one is appropriate based on the context."
  (interactive)
  (cond
   ;; If the cursor is on a plain list item, run org-outdent-item-tree
   ((org-at-item-p) (org-outdent-item-tree))
   ;; If the cursor is on a headline, run org-promote-subtree
   ((org-at-heading-p) (org-promote-subtree))
   ;; Otherwise, do nothing and show a message
   (t (message "Not at an item or a headline"))))

(defun my/org-indent-or-demote ()
  "Run either org-indent-item-tree or org-demote-subtree,
depending on which one is appropriate based on the context."
  (interactive)
  (cond
   ;; If the cursor is on a plain list item, run org-indent-item-tree
   ((org-at-item-p) (org-indent-item-tree))
   ;; If the cursor is on a headline, run org-demote-subtree
   ((org-at-heading-p) (org-demote-subtree))
   ;; Otherwise, do nothing and show a message
   (t (message "Not at an item or a headline"))))

;;;; my/unfill-paragraph-or-region

;; unfill paragraph: the opposite of fill-paragraph
(defun my/unfill-paragraph-or-region (&optional region)
  "Takes a multi-line paragraph and makes it into a single line of text."
  (interactive (progn (barf-if-buffer-read-only) '(t)))
  (let ((fill-column (point-max))
        ;; This would override `fill-column' if it's an integer.
        (emacs-lisp-docstring-fill-column t))
    (fill-paragraph nil region)))

(defun my/unfill-paragraph-keep-formatting (start end)
  "Unfill the region, but preserve plain-text lists and org-mode SCHEDULED tasks."
  (interactive "*r")
  (let ((fill-column (point-max)))
    (unfill-paragraph start end)))

(defun should-unfill-p (start end)
  "Determine whether the region should be unfilled."
  (save-excursion
    (goto-char start)
    (not (or (looking-at "^\\s-*\\([-*+]\\|[0-9]+[.)]\\)\\s-+") ;; plain-text list
             (looking-at "^\\s-*SCHEDULED:")           ;; org-mode SCHEDULED task
             (looking-at "^\\s-*DEADLINE:")            ;; org-mode DEADLINE
             ))))

(defun my/unfill-region-smart (start end)
  "Unfill the region, but preserve plain-text lists and org-mode SCHEDULED tasks."
  (interactive "*r")
  (let ((pos start))
    (while (< pos end)
      (let ((next-pos (or (next-single-property-change pos 'hard) end)))
        (when (should-unfill-p pos next-pos)
          (unfill-region-keep-formatting pos next-pos))
        (setq pos next-pos)))))

;;;; my/region-to-numbered-list

(defun my/region-to-numbered-list (start end)
  "Turn a region into a numbered list."
  (interactive "r")
  (let* ((s (buffer-substring-no-properties start end))
         (split-strings (split-string s "\\([0-9]+\\. \\)" t))
         (trimmed-strings (mapcar 'string-trim split-strings))
         (filtered-strings (cl-remove-if (lambda (x) (string= x "")) trimmed-strings))
         (numbered-strings
          (cl-loop for str in filtered-strings and i from 1
                   collect (concatenate 'string (number-to-string i) ". " str))))
    (delete-region start end)
    (insert (mapconcat 'identity numbered-strings "\n"))))

;;;; my/extract-hyperlinks-from-file

(defun my/extract-hyperlinks-from-file (file)
  "Extracts all hyperlinks from the text file FILE."
  (interactive "fEnter file to extract hyperlinks from: ")
  (let ((hyperlinks '()))
    (with-temp-buffer
      (insert-file-contents file)
      (goto-char (point-min))
      (while (re-search-forward "\\(http\\|https\\|id\\)://[^[:space:]]+" nil t)
        (push (match-string-no-properties 0) hyperlinks)))
    (message "Hyperlinks extracted: %s" hyperlinks)
    hyperlinks))

;;;; my/extract-hyperlinks-from-buffer

(defun my/extract-hyperlinks-from-buffer ()
  "Extracts all hyperlinks from the current buffer."
  (interactive)
  (let ((hyperlinks '()))
    (save-excursion
      (goto-char (point-min))
      (while (re-search-forward "\\[\\[id:[^]]+\\]\\]" nil t)
        (push (match-string-no-properties 0) hyperlinks)))
    (message "Hyperlinks extracted: %s" hyperlinks)
    hyperlinks))

;;;; Capitalize level 1 headings use correct rules of capitalizing titles

(defun my/org-titlecase-level-1 ()
  "Convert all Level 1 org-mode headings to title case."
  (interactive)
  (save-excursion
    (goto-char (point-min))
    (while (re-search-forward "^\\* " nil t)
      (titlecase-line))))

;;;; my/iloveyou

(defun my/iloveyou (args)
  (interactive "P")
  (message "%s" (propertize "I love you!" 'Face '(:foreground "red")))
  )

;;;; my/genfile-timestamp

;; 중국어 문장 부호를 인식하도록 문장 끝을 설정합니다. 채우기에서 마침표 뒤에 두
;; 개의 공백을 삽입할 필요가 없습니다.
;; (setq sentence-end "\\([。！？]\\|……\\|[.?!][]\"')}]*\\($\\|[ \t]\\)\\)[ \t\n]*")

;; generate timestamp such as 2016_1031_ for file name
(defun my/genfile-timestamp()
  (concat (format-time-string "%Y%m%d")
          (char-to-string (+ 65 (random 26)))
          (char-to-string (+ 65 (random 26)))
          "_"))

;; self define functions
;; (defun my/imenu-default-goto-function-advice (orig-fun &rest args)
;;   (apply orig-fun args)
;;   (recenter))

(defun my/get-file-line ()
  "Show (and set kill-ring) current file and line"
  (interactive)
  (unless (buffer-file-name)
    (error "No file for buffer %s" (buffer-name)))
  (let ((msg (format "%s::%d"
                     (file-truename (buffer-file-name))
                     (line-number-at-pos))))
    (kill-new msg)
    (message msg)))

(defun my/get-file-link ()
  "Show (and set kill-ring) current file"
  (interactive)
  (unless (buffer-file-name)
    (error "No file for buffer %s" (buffer-name)))
  (let ((msg (format "file:\\\\%s"
                     (replace-regexp-in-string "/" "\\\\"
                                               (file-truename (buffer-file-name))))))
    (kill-new msg)
    (message msg)))

(defun my/encode-buffer-to-utf8 ()
  "Sets the buffer-file-coding-system to UTF8."
  (interactive)
  (set-buffer-file-coding-system 'utf-8 nil))

(defun my-blink(begin end)
  "blink a region. used for copy and delete"
  (interactive)
  (let* ((rh (make-overlay begin end)))
    (progn
      (overlay-put rh 'face '(:background "DodgerBlue" :foreground "White"))
      (sit-for 0.2 t)
      (delete-overlay rh)
      )))

(defun get-point (symbol &optional arg)
  "get the point"
  (funcall symbol arg)
  (point)
  )

(defun copy-thing (begin-of-thing end-of-thing &optional arg)
  "Copy thing between beg & end into kill ring. Remove leading and
    trailing whitespace while we're at it. Also, remove whitespace before
    column, if any. Also, font-lock will be removed, if any. Also, the
    copied region will be highlighted shortly (it 'blinks')."
  (save-excursion
    (let* ((beg (get-point begin-of-thing 1))
           (end (get-point end-of-thing arg)))
      (progn
        (copy-region-as-kill beg end)
        (with-temp-buffer
          (yank)
          (goto-char 1)
          (while (looking-at "[ \t\n\r]")
            (delete-char 1))
          (delete-trailing-whitespace)
          (delete-whitespace-rectangle (point-min) (point-max)) ;; del column \s, hehe
          (font-lock-unfontify-buffer) ;; reset font lock
          (kill-region (point-min) (point-max))
          )
        ))))

(defun my/copy-word (&optional arg)
  "Copy word at point into kill-ring"
  (interactive "P")
  (my-blink (get-point 'backward-word 1) (get-point 'forward-word 1))
  (copy-thing 'backward-word 'forward-word arg)
  (message "word at point copied"))

(defun my/copy-line (&optional arg)
  "Copy line at point into kill-ring, truncated"
  (interactive "P")
  (my-blink (get-point 'beginning-of-line 1) (get-point 'end-of-line 1))
  (copy-thing 'beginning-of-line 'end-of-line arg)
  (message "line at point copied"))

(defun my/copy-paragraph (&optional arg)
  "Copy paragraph at point into kill-ring, truncated"
  (interactive "P")
  (my-blink (get-point 'backward-paragraph 1) (get-point 'forward-paragraph 1))
  (copy-thing 'backward-paragraph 'forward-paragraph arg)
  (message "paragraph at point copied"))

(defun my/copy-buffer(&optional arg)
  "Copy the whole buffer into kill-ring, as-is"
  (interactive "P")
  (progn
    (my-blink (point-min) (point-max))
    (copy-region-as-kill (point-min) (point-max))
    (message "buffer copied")))

;;;; my/rename-file-and-buffer

;; copy from http://stackoverflow.com/questions/384284/how-do-i-rename-an-open-file-in-emacs
;; http://emacsredux.com/blog/2013/05/04/rename-file-and-buffer/
;; many thanks to Bozhidar Batsov (https://github.com/bbatsov)
(defun my/rename-file-and-buffer ()
  "Rename the current buffer and file it is visiting. Binded to
  key C-c r"
  (interactive)
  (let ((filename (buffer-file-name)))
    (if (not (and filename (file-exists-p filename)))
        (message "Buffer is not visiting a file!")
      (let ((new-name (read-file-name "New name: " filename)))
        (cond
         ((vc-backend filename) (vc-rename-file filename new-name))
         (t
          (rename-file filename new-name t)
          (set-visited-file-name new-name t t)))))))

(defadvice grep-compute-defaults (around grep-compute-defaults-advice-null-device)
  "Use cygwin's /dev/null as the null-device."
  (let ((null-device "/dev/null"))
    ad-do-it))
(ad-activate 'grep-compute-defaults)
(setq grep-find-command
      "find . -type f -not -name \"*.svn-base\" -and -not -name \"*#\" -and -not -name \"*.tmp\" -and -not -name \"*.obj\" -and -not -name \"*.386\" -and -not -name \"*.img\" -and -not -name \"*.LNK\" -and -not -name GTAGS -print0 | xargs -0 grep -n -e ")

(defun my/grep-find()
  (interactive)
  (grep-find (concat grep-find-command (buffer-substring-no-properties (region-beginning) (region-end)))))

;;;; my/open-external

(defun my/open-external (&optional file)
  "Open the current file or dired marked files in external app.
The app is chosen from your OS's preference.
copy from xah lee: http://ergoemacs.org/emacs/emacs_dired_open_file_in_ext_apps.html"
  (interactive)
  (let (doit
        (flist
         (cond
          ((or (string-equal major-mode "dired-mode")
               (string-equal major-mode "sr-mode"))
           (dired-get-marked-files))
          ((not file) (list (buffer-file-name)))
          (file (list file)))))

    (setq doit (if (<= (length flist) 5)
                   t
                 (y-or-n-p "Open more than 5 files? ")))
    (when doit
      (cond
       ((string-equal system-type "windows-nt")
        (mapc (lambda (path) (w32-shell-execute "open" (replace-regexp-in-string "/" "\\" path t t)) ) flist))
       ((string-equal system-type "darwin")
        (mapc (lambda (path) (shell-command (format "open \"%s\"" path)))  flist))
       ((string-equal system-type "gnu/linux")
        (mapc (lambda (path) (let ((process-connection-type nil)) (start-process "" nil "xdg-open" path)) ) flist))
       ((string-equal system-type "cygwin")
        (mapc (lambda (path) (let ((process-connection-type nil)) (start-process "" nil "xdg-open" path)) ) flist))))))

;;;; my/open-external-pdf

(defun my/open-external-pdf ()
  (interactive)
  (my/open-external
   (concat
    (file-name-sans-extension (buffer-file-name))
    ".pdf")))

;;;; pkm func

(defun range (n)
  "Python like range function returning list."
  (cl-loop for i from 0 to (- n 1)
           collect i))

(defun shuffle-list (its)
  "Destructive but inefficient list shuffling."
  (cl-loop for i downfrom (- (length its) 1) to 1
           do (let ((i-val (nth i its))
                    (j (random (+ i 1))))
                (setf (nth i its) (nth j its))
                (setf (nth j its) i-val)))
  its)

(defun shuffle-org-list ()
  "Shuffle list at point."
  (interactive)
  (save-excursion
    (let ((org-list (org-list-to-lisp t)))
      (insert (org-list-to-org (cons (car org-list) (shuffle-list (cdr org-list)))))
      (org-list-repair))))

(defun reading-time (&optional wpm)
  (/ (count-words (point-min) (point-max)) (or wpm 200)))

(defun firefox-profile-directory ()
  "Return profile directory for firefox."
  (-find (lambda (d) (string-match "default$" d)) (f-directories "~/.mozilla/firefox")))

(defmacro --with-temp-copy (file-path &rest body)
  "Run BODY after making a temporary copy of given FILE-PATH.

  In the BODY forms, `it' provides the path for the copy."
  (declare (indent defun))
  `(let ((it (make-temp-file (f-base ,file-path))))
     (unwind-protect
         (progn
           (copy-file ,file-path it t)
           ,@body)
       (f-delete it))))

(defun youtube-history ()
  "Return youtube history."
  (--with-temp-copy (f-join (firefox-profile-directory) "places.sqlite")
    (json-parse-string
     (shell-command-to-string
      (format "sqlite3 -json %s %s"
              (shell-quote-argument it)
              (shell-quote-argument "SELECT url, title FROM moz_places WHERE title IS NOT NULL AND rev_host LIKE '%utuoy%' AND url LIKE '%watch%' ORDER BY last_visit_date DESC")))
     :array-type 'list
     :object-type 'alist)))

(defvar youtube-process nil
  "Process for keeping youtube player.")

;; (defun youtube-play-url (url)
;;   (when (and youtube-process (process-live-p youtube-process))
;;     (kill-process youtube-process))
;;   (setq youtube-process (start-process "youtube-play" nil "mpv" "--no-video" url)))

;; (defun youtube-history-play ()
;;   (interactive)
;;   (helm :sources (helm-build-sync-source "youtube-history"
;;                                          :candidates (mapcar (lambda (it) (cons (alist-get 'title it) (alist-get 'url it))) (youtube-history))
;;                                          :action `(("Play audio" . youtube-play-url)))
;;         :buffer "*helm youtube history*"
;;         :prompt "Title: "))

(defun aleatory-assitance ()
  "Give a random strategy to get unstuck."
  (interactive)
  (let ((strategies (s-split "\n" (s-trim "1. Take the braver decision
    2. Take a nap
    3. What's the title of this book?
    4. What's the choice between?
    5. Ask ChatGPT for the final decision
    6. What will make you proud of yourself?
    7. Choose freedom
    8. Start reading a new book
    9. Be kind to people involved
    10. Name this
    11. Start a repository
    12. Where's the money coming from?
    13. Close everything, start again
    14. Connect with an expert in the area
    15. How would you have done it?
    16. Combine two unrelated concepts
    17. Toss a coin
    18. Explain it to a business person
    19. How much time will it take? Take 3 times more
    20. Talk to the nearest human
    21. What are the ingredients? What's missing?
    22. Search old notes
    23. How will this look like in the future?
    24. Find an equivalent problem
    25. Work in a different domain
    26. Take out another card
    27. What's the most ambitious option?
    28. List risks
    29. Run an experiment
    30. Record a video on current status
    31. Ship right now!
    32. What's the strongest feeling right now?
    33. What's one bias you can remove right now?
    34. Write an email
    35. What's the weather trend these days?
    36. Use a new animal
    37. Make a plot
    38. Collect data
    39. Make it efficient
    40. Remove the most meaningless portion
    41. Make a mistake
    42. What do you need other than time? Ask for it.
    "))))
    (print (car (shuffle-list strategies)))))

(defun org-hide-properties ()
  "Hide all org-mode headline property drawers in buffer. Could be slow if it has a lot of overlays."
  (interactive)
  (save-excursion
    (goto-char (point-min))
    (while (re-search-forward
            "^ *:properties:\n\\( *:.+?:.*\n\\)+ *:end:\n" nil t)
      (let ((ov_this (make-overlay (match-beginning 0) (match-end 0))))
        (overlay-put ov_this 'display "")
        (overlay-put ov_this 'hidden-prop-drawer t))))
  (put 'org-toggle-properties-hide-state 'state 'hidden))

(defun org-show-properties ()
  "Show all org-mode property drawers hidden by org-hide-properties."
  (interactive)
  (remove-overlays (point-min) (point-max) 'hidden-prop-drawer t)
  (put 'org-toggle-properties-hide-state 'state 'shown))

(defun org-toggle-properties ()
  "Toggle visibility of property drawers."
  (interactive)
  (if (eq (get 'org-toggle-properties-hide-state 'state) 'hidden)
      (org-show-properties)
    (org-hide-properties)))

;; (setq my_shell_output
;;       (substring
;;        (shell-command-to-string "/bin/echo hello")
;;        0 -1))

(defun my/find-random-file-old-journals ()
  (interactive)
  (find-file
   (substring
    (shell-command-to-string "find ~/org/roam/oldseq/journals -type f | shuf -n 1")
    0 -1)))

(defun my/find-random-file-old-pages ()
  (interactive)
  (find-file
   (substring
    (shell-command-to-string "find ~/org/roam/oldseq/pages -type f | shuf -n 1")
    0 -1)))

;;;; clone-notes
(defun my/rg-search-clone-notes-all ()
  "Search org-roam directory using consult-ripgrep. With live-preview."
  (interactive)
  (let ((consult-ripgrep-command "rg --ignore-case --type org --type md --line-buffered --color=always --max-columns=500 --no-heading --line-number . -e ARG OPTS"))
    (consult-ripgrep "~/nosync/clone-notes/")))

(defun my/rg-search-clone-notes-ko ()
  "Search org-roam directory using consult-ripgrep. With live-preview."
  (interactive)
  (let ((consult-ripgrep-command "rg --ignore-case --type org --type md --line-buffered --color=always --max-columns=500 --no-heading --line-number . -e ARG OPTS"))
    (consult-ripgrep "~/nosync/clone-notes/ko/")))

;;;; my notes

(defun my/org-roam-rg-file-search ()
  "Search org-roam directory using consult-find with \"rg --file-with-matches \". No live preview."
  (interactive)
  (my/consult-rg-files-with-matches org-roam-directory))

(defun my/consult-rg-files-with-matches (&optional dir initial)
  "Use consult-find style to return matches with \"rg --file-with-matches \". No live preview."
  (interactive "P")
  (let ((consult-find-command "rg --ignore-case --type org --files-with-matches . -e ARG OPTS"))
    (consult-find dir initial)))

(defun my/consult-rg-files-without-matches (&optional dir initial)
  "Use consult-find style to return matches with \"rg --file-without-matches \". No live preview."
  (interactive "P")
  (let ((consult-find-command "rg --ignore-case --type org --files-without-matches . -e ARG OPTS"))
    (consult-find dir initial)))

;;;###autoload
(defun my/consult-org-agenda-heading (&optional match)
  "Jump to an Org heading.
MATCH is as in org-map-entries and determine which
entries are offered."
  (interactive)
  (consult-org-heading match '(directory-files-recursively org-workflow-directory "\\.org")))




;;; gr-functions
;;;; functions

;;;###autoload
(defun gr/daily-notes (p)
  "Pop up dailynotes.org."
  (interactive "P")
  (let ((buffer (find-file-noselect
                 (concat org-directory "/dailynotes.org"))))
    (cond ((equal p '(4))
           (select-frame (make-frame-command))
           (find-file (concat org-directory "/dailynotes.org"))
           (set-frame-position (selected-frame) 845 20)
           (delete-other-windows))
          ((eq (current-buffer) buffer)
           nil)
          (t (pop-to-buffer-same-window buffer)))
    (gr/daily-notes-new-headline)))

(defun gr/daily-notes-new-headline ()
  (interactive)
  (org-cycle-set-startup-visibility)
  (let ((date (concat "** " (format-time-string "%Y-%m-%d %A")))
        (month (concat "* " (format-time-string "%B %Y")))
        (last-month (format-time-string "%B"
                                        (time-subtract
                                         (current-time)
                                         (days-to-time 30)))))
    (goto-char (point-min))
    (unless (re-search-forward month nil t)
      (re-search-forward (concat "* " last-month))
      (forward-line 1)
      (kill-whole-line)
      (forward-line -1)
      (org-cycle)
      (insert month "\n\n")
      (forward-line -2)
      (org-set-property "VISIBILITY" "all"))
    (unless (re-search-forward date nil t)
      (forward-line 4)
      (insert "\n" date "\n- |\n")
      (search-backward "|")
      (delete-char 1))))

(defun gr/word-count-subtree ()
  "Count words in org subtree at point."
  (interactive)
  (save-restriction
    (org-narrow-to-subtree)
    (let ((wc (org-word-count-aux (point-min) (point-max))))
      (kill-new (format "%d" wc))
      (message (format "%d words in subtree." wc)))))

;; (defun gr/org-journal-new-entry ()
;;   (interactive)
;;   (select-frame (make-frame-command))
;;   (delete-other-windows)
;;   (org-journal-new-entry nil nil))

(defun gr/org-journal-new-entry ()
  (interactive)
  (split-window-below)
  (windmove-down)
  (org-journal-new-entry nil nil)
  (goto-char (point-max)))

(defun gr/lookup-word-at-point ()
  "Lookup word at point in OSX Dictionary."
  (interactive)
  (call-process-shell-command (format "open dict:///%s/" (word-at-point))))

;; (defun gr/open-mu4e ()
;;   (interactive)
;;   (select-frame (make-frame))
;;   (set-frame-size (selected-frame) 125 45)
;;   (mu4e)
;;   (delete-other-windows))
;;   (display-buffer-full-frame " *mu4e-main*" '(nil)))
;;   ;; (switch-to-buffer " *mu4e-main*")
;;   (pop-to-buffer-same-window " *mu4e-main*")
;;   (delete-other-windows))
;; ;; use display-buffer-full-frame ?

(defun gr/open-fragments-file ()
  (interactive)
  (find-file "~/Dropbox/org/fragments.org"))

;; (defun gr/open-fragments-file-other-frame ()
;;   (interactive)
;;   (find-file-other-frame "~/Dropbox/org/fragments.org"))

;; (defun switch-to-minibuffer-window ()
;;   "Switch to minibuffer window (if active)."
;;   (interactive)
;;   (when (active-minibuffer-window)
;;     (select-window (active-minibuffer-window))))

;; load ~/.emacs.d/init.el
;; (defun refresh-emacs ()
;;   (interactive)
;;   (load "~/.emacs.d/init.el"))

;; (global-set-key (kbd "C-c I") 'refresh-emacs)

;; (defun gr/open-tasks-file ()
;;   (interactive)
;;   (find-file "~/Dropbox/org/tasks.org"))

;; (defun gr/open-tasks-upcoming-agenda ()
;;   (interactive)
;;   (find-file "~/Dropbox/org/tasks.org")
;;   (delete-other-windows)
;;   (set-frame-size (selected-frame) 80 43)
;;   (split-window-below)
;;   (org-agenda nil "y")
;;   (other-window 1)
;;   (enlarge-window 5))

(defun gr/open-inbox-below ()
  (interactive)
  (let ((buffer (find-file-noselect org-refile-file)))
    (pop-to-buffer buffer
                   '(display-buffer-at-bottom))))

;; (defun gr/open-tasks-upcoming-agenda-other-frame ()
;;   (interactive)
;;   (find-file-other-frame "~/Dropbox/org/tasks.org")
;;   (set-frame-size (selected-frame) 80 43)
;;   (delete-other-windows)
;;   (split-window-below)
;;   (org-agenda nil "y")
;;   (other-window 1)
;;   (enlarge-window 5)
;;   )

;; (defun gr/open-init-file (p)
;;   "Open myinit.org in new frame. With universal argument, open in current window."
;;   (interactive "P")
;;   (cond ((equal p '(4))
;;           (gr/make-frame)
;;           (find-file (concat user-emacs-directory "init.el")))
;;     (t (find-file (concat user-emacs-directory "init.el")))))

;;OPEN init.el
(defun gr/create-frame-with-init-file ()
  "Open ~/.emacs.d/init.el in new frame."
  (interactive)
  (find-file-other-frame dotspacemacs-filepath)
  (set-frame-position (selected-frame) 845 20))

;; OPEN New Frame
;; (defun gr/new-window ()
;;   (interactive)
;;   (select-frame (make-frame-command))
;;   (set-frame-position (selected-frame) 200 100)
;;   (delete-other-windows))

;; (defun gr/toggle-capslock ()
;;   "Toggle capslock.
;; See bin in ~/Repos/capslock and source
;; https://discussions.apple.com/thread/7094207"
;;   (interactive)
;;   (shell-command "capslock -1"))
;; (keymap-global-set "<f12>" 'gr/toggle-capslock)

(defun gr/insert-line (p)
  (interactive "P")
  (cond ((equal p '(4)) (save-excursion
                          (end-of-line 0)
                          (open-line 1)))
        (t (save-excursion
             (end-of-line)
             (open-line 1)))))

;; (global-set-key (kbd "C-o") 'gr/insert-line)

(defun gr/comment-and-copy ()
  (interactive)
  (let ((beg (region-beginning))
        (end (region-end)))
    (kill-ring-save beg end t)
    (comment-region beg end)
    (goto-char end)
    (forward-line 2)
    (save-excursion
      (yank)
      (newline 2))))

;; (bind-key* (kbd "C-M-;") 'gr/comment-and-copy)

(defun gr/copy-file-path ()
  "Copy the current buffer file path to the clipboard."
  (interactive)
  (let ((filepath (if (equal major-mode 'dired-mode)
                      default-directory
                    (buffer-file-name))))
    (when filepath
      (kill-new filepath)
      (message "Copied buffer file path '%s' to the clipboard." filepath))))

(defun gr/copy-file-name ()
  "Copy the current buffer file name to the clipboard."
  (interactive)
  (let ((filename (if (equal major-mode 'dired-mode)
                      default-directory
                    (buffer-name))))
    (when filename
      (kill-new filename)
      (message "Copied buffer file name '%s' to the clipboard." filename))))

;;;; capitalize, upcase, downcase dwim

(defun title-case-region ()
  "Render string in region in title case."
  (interactive)
  (let* ((input (buffer-substring (region-beginning)
                                  (region-end)))
         (words (split-string input))
         (first (pop words))
         (last (car(last words)))
         (do-not-capitalize '("a" "an" "and" "as" "at" "but" "by" "en" "for" "if" "in" "of" "on" "or" "the" "to" "via"))
         (output (concat (capitalize first)
                         " "
                         (mapconcat (lambda (w)
                                      (if (not(member (downcase w) do-not-capitalize))
                                          (capitalize w)(downcase w)))
                                    (butlast words) " ")
                         " " (capitalize last))))
    (replace-string input output nil (region-beginning)(region-end))))


(defun ct/word-boundary-at-point-or-region (&optional callback)
  "Return the boundary (beginning and end) of the word at point, or region, if any.
Forwards the points to CALLBACK as (CALLBACK p1 p2), if present.

URL: https://christiantietze.de/posts/2021/03/change-case-of-word-at-point/"
  (let ((deactivate-mark nil)
        $p1 $p2)
    (if (use-region-p)
        (setq $p1 (region-beginning)
              $p2 (region-end))
      (save-excursion
        (skip-chars-backward "[:alpha:]")
        (setq $p1 (point))
        (skip-chars-forward "[:alpha:]")
        (setq $p2 (point))))
    (when callback
      (funcall callback $p1 $p2))
    (list $p1 $p2)))

;; (defun ct/capitalize-word-at-point ()
;;   (interactive)
;;   (ct/word-boundary-at-point-or-region #'upcase-initials-region))

(defun ct/downcase-word-at-point ()
  (interactive)
  (ct/word-boundary-at-point-or-region #'downcase-region))

(defun ct/upcase-word-at-point ()
  (interactive)
  (ct/word-boundary-at-point-or-region #'upcase-region))

(defun ct/capitalize-region (p1 p2)
  (downcase-region p1 p2)
  (upcase-initials-region p1 p2))

(defun ct/capitalize-word-at-point ()
  (interactive)
  (ct/word-boundary-at-point-or-region #'ct/capitalize-region))

;; Set global shortcuts
;; (global-set-key (kbd "M-c") #'ct/capitalize-word-at-point)
;; (global-set-key (kbd "M-u") #'ct/upcase-word-at-point)
;; (global-set-key (kbd "M-l") #'ct/downcase-word-at-point)
;; (global-set-key (kbd "M-U") #'title-case-region)



;;;; convert docx to org

(defun gr/flush-properties-drawers ()
  (interactive)
  (goto-line 2)
  (flush-lines ":PROPERTIES:")
  (flush-lines ":CUSTOM_ID:")
  (flush-lines ":END:")
  )

(defun gr/convert-pandoc-docx-org ()
  "Use pandoc via shell command to convert a docx file to an org file.
Navigate to files in dired, mark files, and execute command."
  (interactive)
  (dired-do-async-shell-command
   "pandoc -f docx -t org --wrap=none" current-prefix-arg
   (dired-get-marked-files t current-prefix-arg))
  (switch-to-buffer-other-window "*Async Shell Command*")
  (run-with-idle-timer 1 nil
                       'gr/flush-properties-drawers)
  (goto-line 2)
  (run-with-idle-timer 1 nil
                       'gr/flush-properties-drawers)
  )

;;;; "Better Return" edited

;; a better return; inserts list item with RET instead of M-RET

(defun scimax/org-return (&optional ignore)
  "Add new list item, heading or table row with RET.
A double return on an empty element deletes it.
Use a prefix arg to get regular RET. "
  (interactive "P")
  (if ignore
      (org-return)
    (cond

     ((eq 'line-break (car (org-element-context)))
      (org-return t))

     ;; Open links like usual, unless point is at the end of a line.
     ;; and if at beginning of line, just press enter.
     ((or (and (eq 'link (car (org-element-context))) (not (eolp)))
          (bolp))
      (org-return))

     ;; checkboxes - add new or delete empty
     ((org-at-item-checkbox-p)
      (cond
       ;; at the end of a line.
       ((and (eolp)
             (not (eq 'item (car (org-element-context)))))
        (org-insert-todo-heading nil))
       ;; no content, delete
       ((and (eolp) (eq 'item (car (org-element-context))))
        (delete-region (line-beginning-position) (line-end-position)))
       ((eq 'paragraph (car (org-element-context)))
        (goto-char (org-element-property :end (org-element-context)))
        (org-insert-todo-heading nil))
       (t
        (org-return))))

     ;; lists end with two blank lines, so we need to make sure we are also not
     ;; at the beginning of a line to avoid a loop where a new entry gets
     ;; created with only one blank line.
     ((org-in-item-p)
      (cond
       ;; empty definition list
       ((and (looking-at " ::")
             (looking-back "- " 3))
        (beginning-of-line)
        (delete-region (line-beginning-position) (line-end-position)))
       ;; empty item
       ((and (looking-at "$")
             (or
              (looking-back "- " 3)
              (looking-back "+ " 3)
              (looking-back " \\* " 3)))
        (beginning-of-line)
        (delete-region (line-beginning-position) (line-end-position)))
       ;; numbered list
       ((and (looking-at "$")
             (looking-back "^[0-9]+. " (line-beginning-position)))
        (beginning-of-line)
        (delete-region (line-beginning-position) (line-end-position)))
       ;; insert new item
       (t
        (if (not (looking-at "$"))
            (org-return)
          (end-of-line)
          (org-insert-item)))))

     ;; org-heading
     ((org-at-heading-p)
      (if (not (string= "" (org-element-property :title (org-element-context))))
          (if (not (looking-at "$"))
              (org-return)
            (progn
              ;; Go to end of subtree suggested by Pablo GG on Disqus post.
              ;;(org-end-of-subtree)
              (org-meta-return)
              ;;(org-metaright)
              ;;(org-insert-heading-respect-content)
              (outline-show-entry)
              ))
        ;; The heading was empty, so we delete it
        (beginning-of-line)
        (delete-region (line-beginning-position) (line-end-position))))

     ;; tables
     ((org-at-table-p)
      (if (-any?
           (lambda (x) (not (string= "" x)))
           (nth
            (- (org-table-current-dline) 1)
            (remove 'hline (org-table-to-lisp))))
          (org-return)
        ;; empty row
        (beginning-of-line)
        (delete-region (line-beginning-position) (line-end-position))
        (org-return)))

     ;; footnotes
     ((or (org-footnote-at-reference-p)
          (org-footnote-at-definition-p))
      (org-footnote-action))

     ;; fall-through case
     (t
      (org-return)))))

(provide 'gr-functions)
;;; karthink-dotfiles-popper/lisp/utilities.el

;;;; COUNT-WORDS-REGION: USING `while'

;; ;;;###autoload
(defun my/count-words-region (beginning end)
  "Print number of words in the region."
  (interactive "r")
  (message "Counting words in region ... ")
  ;; 1. Set up appropriate conditions.
  (save-excursion
    (let ((count 0))
      (goto-char beginning)
      ;; 2. Run the while loop.
      (while (and (< (point) end)
		          (re-search-forward "\\w+\\W*" end t))
	    (setq count (1+ count)))
      ;; 3. Send a message to the user.
      (cond ((zerop count)
	         (message
	          "The region does NOT have any words."))
	        ((= 1 count)
	         (message
	          "The region has 1 word."))
	        (t
	         (message
	          "The region has %d words." count))))))

;; count words in region
;; (global-set-key (kbd "C-=") 'my/count-words-region)

;; ;;;###autoload
(defun my/count-words-buffer ()
  "Print number of words in the region."
  ;; 1. Set up appropriate conditions.
  (save-excursion
    (let ((count 0)
          (beginning (point-min))
          (end (point-max)))
      (goto-char beginning)
      ;; 2. Run the while loop.
      (while (and (< (point) end)
		          (re-search-forward "\\w+\\W*" end t))
	    (setq count (1+ count)))
      ;; 3. Send a message to the user.
      (cond ((zerop count) (message "No words"))
            ((= 1 count) (message "1 word"))
            (t  (message  (format "%d words" count)))))))

;;;; PRINT ASCII TABLE

;; ;;;###autoload
(defun ascii-table ()
  "Display basic ASCII table (0 thru 127)"
  (interactive)
  (pop-to-buffer "*ASCII*")
  (erase-buffer)
  (save-excursion (let ((i -1))
                    (insert "ASCII characters 0 thru 127.\n\n")
                    (insert " Hex  Dec  Char|  Hex  Dec  Char|  Hex  Dec  Char|  Hex  Dec  Char\n")
                    (while (< i 31)
                      (insert (format "%4x %4d %4s | %4x %4d %4s | %4x %4d %4s | %4x %4d %4s\n"
                                      (setq i (+ 1  i)) i (single-key-description i)
                                      (setq i (+ 32 i)) i (single-key-description i)
                                      (setq i (+ 32 i)) i (single-key-description i)
                                      (setq i (+ 32 i)) i (single-key-description i)))
                      (setq i (- i 96)))))
  (special-mode))

;;;; INSERT FUNCTION DEFINITION AT POINT

;; ;;;###autoload
(defun insert-definition-at-point ()
  "Function to find the definition of the defun at point and insert it there."
  (interactive)
  (save-excursion
    (imenu (thing-at-point 'symbol))
    (mark-defun)
    (kill-ring-save (region-beginning)
                    (region-end)))
  (with-temp-buffer
    (yank)
    (beginning-of-buffer)
    (delete-blank-lines)
    (kill-new (buffer-substring-no-properties
               (point-min)
               (point-max))
              t))
  (beginning-of-line)
  (yank)
  (kill-whole-line)
  (beginning-of-defun))
;; (global-set-key (kbd "C-x C-M-y") 'insert-definition-at-point)

;;; Denote

;;;; ews-org-insert-notes-drawer

;; Notes drawers
(defun ews-org-insert-notes-drawer ()
  "Generate a NOTES drawer under the heading of the current or jump to an existing one."
  (interactive)
  (push-mark)
  (org-previous-visible-heading 1)
  (next-line 1)
  (if (looking-at-p "^[ \t]*:NOTES:")
      (progn
        (re-search-forward "^[ \t]*:END:" nil t)
        (previous-line)
        (end-of-line)
        (org-return))
    (org-insert-drawer nil "NOTES"))
  (message "Press C-u C-SPACE to return to previous position."))

;;;; 13.3. Split an Org subtree into its own note
(defun my-denote-org-extract-subtree (&optional silo)
  "Create new Denote note using current Org subtree.
Make the new note use the Org file type, regardless of the value
of `denote-file-type'.

With an optional SILO argument as a prefix (\\[universal-argument]),
ask user to select a SILO from `my-denote-silo-directories'.

Use the subtree title as the note's title.  If available, use the
tags of the heading are used as note keywords.

Delete the original subtree."
  (interactive
   (list (when current-prefix-arg
           (completing-read "Select a silo: " my-denote-silo-directories nil t))))
  (if-let ((text (org-get-entry))
           (heading (org-get-heading :no-tags :no-todo :no-priority :no-comment)))
      (let ((element (org-element-at-point))
            (tags (org-get-tags))
            (denote-user-enforced-denote-directory silo))
        (delete-region (org-entry-beginning-position)
                       (save-excursion (org-end-of-subtree t) (point)))
        (denote heading
                tags
                'org
                nil
                (or
                 ;; Check PROPERTIES drawer for :created: or :date:
                 (org-element-property :CREATED element)
                 (org-element-property :DATE element)
                 ;; Check the subtree for CLOSED
                 (org-element-property :raw-value
                                       (org-element-property :closed element))))
        (insert text))
    (user-error "No subtree to extract; aborting")))


;;;; 13.4. =Narrow the list= of files in Dired
(defvar prot-dired--limit-hist '()
  "Minibuffer history for `prot-dired-limit-regexp'.")

;;;###autoload
(defun prot-dired-limit-regexp (regexp omit)
  "Limit Dired to keep files matching REGEXP.

With optional OMIT argument as a prefix (\\[universal-argument]),
exclude files matching REGEXP.

Restore the buffer with \\<dired-mode-map>`\\[revert-buffer]'."
  (interactive
   (list
    (read-regexp
     (concat "Files "
             (when current-prefix-arg
               (propertize "NOT " 'face 'warning))
             "matching PATTERN: ")
     nil 'prot-dired--limit-hist)
    current-prefix-arg))
  (dired-mark-files-regexp regexp)
  (unless omit (dired-toggle-marks))
  (dired-do-kill-lines))

;;;; 13.5. Use =dired-virtual-mode= for arbitrary file listings

(defcustom prot-eshell-output-buffer "*Exported Eshell output*"
  "Name of buffer with the last output of Eshell command.
Used by `prot-eshell-export'."
  :type 'string
  :group 'eshell)

(defcustom prot-eshell-output-delimiter "* * *"
  "Delimiter for successive `prot-eshell-export' outputs.
This is formatted internally to have newline characters before
and after it."
  :type 'string
  :group 'eshell)

(defun prot-eshell--command-prompt-output ()
  "Capture last command prompt and its output."
  (let ((beg (save-excursion
               (goto-char (eshell-beginning-of-input))
               (goto-char (point-at-bol)))))
    (when (derived-mode-p 'eshell-mode)
      (buffer-substring-no-properties beg (eshell-end-of-output)))))

;;;###autoload
(defun prot-eshell-export ()
  "Produce a buffer with output of the last Eshell command.
If `prot-eshell-output-buffer' does not exist, create it.  Else
append to it, while separating multiple outputs with
`prot-eshell-output-delimiter'."
  (interactive)
  (let ((eshell-output (prot-eshell--command-prompt-output)))
    (with-current-buffer (get-buffer-create prot-eshell-output-buffer)
      (let ((inhibit-read-only t))
        (goto-char (point-max))
        (unless (eq (point-min) (point-max))
          (insert (format "\n%s\n\n" prot-eshell-output-delimiter)))
        (goto-char (point-at-bol))
        (insert eshell-output)
        (switch-to-buffer-other-window (current-buffer))))))

;;;; efls/denote-signature-buffer

(defun efls/denote-signature-buffer ()
  (interactive)
  (switch-to-buffer "*denote-signatures*")
  (read-only-mode -1)
  (erase-buffer)
  (insert
   (shell-command-to-string
    "ls -l | awk /==/ | sed  's/--/=@/3' | sort -t '=' -Vk 3,3 | sed 's/=@/--/' "))
  (dired-virtual denote-directory)
  (denote-dired-mode)
  (auto-revert-mode -1))

;;; CUSTOM-ID
;; https://writequit.org/articles/emacs-org-mode-generate-ids.html

;; Then we can define our own version of org-custom-id-get that calls org-id-new and creates a new property if one doesn't already exist
;; 그런 다음 org-id-new 을 호출하고 프로퍼티가 없는 경우 새 프로퍼티를 생성하는 org-custom-id-get 의 자체 버전을 정의할 수 있습니다.
;; (require 'org-id)
;; (setq org-id-link-to-org-use-id 'create-if-interactive-and-no-custom-id)

;; Copied from this article (with minor tweaks from my side):
;; <https://writequit.org/articles/emacs-org-mode-generate-ids.html>.
(defun prot-org--id-get (&optional pom create prefix)
  "Get the CUSTOM_ID property of the entry at point-or-marker POM.

If POM is nil, refer to the entry at point.  If the entry does
not have an CUSTOM_ID, the function returns nil.  However, when
CREATE is non nil, create a CUSTOM_ID if none is present already.
PREFIX will be passed through to `org-id-new'.  In any case, the
CUSTOM_ID of the entry is returned."
  (org-with-point-at pom
    (let ((id (org-entry-get nil "CUSTOM_ID")))
      (cond
       ((and id (stringp id) (string-match "\\S-" id))
        id)
       (create
        (setq id (org-id-new (concat prefix "h")))
        (org-entry-put pom "CUSTOM_ID" id)
        (org-id-add-location id (format "%s" (buffer-file-name (buffer-base-buffer))))
        id)))))

;; And add a helper function that's interactive to add custom ids to all headlines in the buffer if they don't already have one.
;; 또한 버퍼의 모든 헤드라인에 사용자 지정 아이디가 없는 경우 대화형 도우미 기능을 추가하여 사용자 지정 아이디를 추가할 수 있습니다.

;;;###autoload
(defun prot-org-id-headlines ()
  "Add missing CUSTOM_ID to all headlines in current file."
  (interactive)
  (org-map-entries
   (lambda () (prot-org--id-get (point) t))))

;;;###autoload
(defun prot-org-id-headline ()
  "Add missing CUSTOM_ID to headline at point."
  (interactive)
  (prot-org--id-get (point) t))

;;; Export

;;;###autoload
(defun prot-org-ox-html ()
  "Streamline HTML export."
  (interactive)
  (org-html-export-as-html nil nil nil t nil))

;;;###autoload
(defun prot-org-ox-texinfo ()
  "Streamline Info export."
  (interactive)
  (org-texinfo-export-to-info))

;;; my-custom func

;; simple and replace text without org-element-parse-buffer

;; .emacs.d/lisp/dw-org.el

;; (org-element-context)
;; (setq title (org-get-title))

;; 타이틀 얻어오고 타임얻어와서 파일 이름 만들어 주면 된다.
;; (org-element-parse-buffer)

;; (org-set-property-and-value ...)

;; (defun org-get-identifier (&optional buffer-or-file)
;;   "Collect title from the provided `org-mode' BUFFER-OR-FILE.

;; Returns nil if there are no #+TITLE property."
;;   (let ((buffer (cond ((bufferp buffer-or-file) buffer-or-file)
;;                       ((stringp buffer-or-file) (find-file-noselect
;;                                                  buffer-or-file))
;;                       (t (current-buffer)))))
;;     (with-current-buffer buffer
;;       (org-macro-initialize-templates)
;;       (let ((title (assoc-default "identifier" org-macro-templates)))
;;         (unless (string= "" title)
;;           title)))))


(defun my/rename-file-and-buffer ()
  "Rename the current buffer and file it is visiting. Binded to
  key C-c r"
  (interactive)
  (let ((filename (buffer-file-name)))
    (if (not (and filename (file-exists-p filename)))
        (message "Buffer is not visiting a file!")
      (let ((new-name (read-file-name "New name: " filename)))
        (cond
         ((vc-backend filename) (vc-rename-file filename new-name))
         (t
          (rename-file filename new-name t)
          (set-visited-file-name new-name t t)))))))

;;;; denote

(defun open-dired-denote-directory ()
  "Open dired at denote-directory"
  (interactive)
  (dired denote-directory))

;; #+date: <2023-08-10 Thu 10:13>
;; #+identifier: 20230810T101300
(defun my-denote-format-time (time-string)
  (let* ((time (parse-time-string time-string))
         (sec (nth 0 time))
         (min (nth 1 time))
         (hour (nth 2 time))
         (day (nth 3 time))
         (month (nth 4 time))
         (year (nth 5 time))
         )
    (format-time-string "%Y%m%dT%H%M%S" ;
                        ;; I would love to do an 'apply here, but the given list to be encoded contains nil which does n
                        (encode-time sec min hour day month year))))

(defun my/org-rename-org-roam-to-denote (directory extension)
  "Set #+filetags in `current-buffer' to TAGS.
Existing filetags aren't removed, but are converted to :tag:
format."

  (interactive (list (read-directory-name "Directory: ")
                     (read-string "File extension: ")))

  (setq lsp-rename-use-prepare nil)

  (dolist (file (directory-files-recursively directory extension))
    (find-file file)

    (goto-char (point-min))
    (re-search-forward (rx "#+date: ") nil t)
    (end-of-line)

    (let* ((el (org-element-context))
           (dt (org-element-property :value el))
           (id (my-denote-format-time dt))
           )

      (insert (format "\n#+identifier: %s\n" id))
      (save-buffer)
      (rename-file (buffer-name)
                   (concat
                    id
                    "--"
                    (buffer-name)
                    ) t)
      (kill-buffer (buffer-name))
      )
    )
  )

;; 2023-11-08 프로퍼티헤더에 빈라인 있으면 인식 안됨. 간단히 해결.
(defun my/org-delete-blank-on-property-headear (directory extension)
  (interactive (list (read-directory-name "Directory: ")
                     (read-string "File extension: ")))

  (dolist (file (directory-files-recursively directory extension))
    (find-file file)
    (goto-line 3)
    (delete-blank-lines)
    (delete-blank-lines)
    (delete-blank-lines)
    (save-buffer)
    (kill-buffer (buffer-name))
    )
  )


;;;; Custom functions

;; (defun my/org-id-gets-files (directory extension)
;;   (interactive (list (read-directory-name "Directory: ")
;;                      (read-string "File extension: ")))
;;   (dolist (file (directory-files-recursively directory extension))
;;     (find-file file)
;;     (goto-line 2)
;;     (newline)
;;     (goto-line 2)
;;     (insert "#+filetags: :@ARCHIVE:@LOGSEQ:\n")
;;     (org-id-get-create)
;;     (save-buffer)
;;     (kill-buffer nil))
;;   )

;; (defun my/rename-for-denote-using-front-matter (directory extension)
;;   (interactive (list (read-directory-name "Directory: ")
;;                      (read-string "File extension: ")))
;;   (dolist (file (directory-files-recursively directory extension))
;;     ;; (find-file file)
;;     ;; (denote-rename-file-using-front-matter file t)
;;     ;; (insert "#+filetags: :@ARCHIVE:@LOGSEQ:\n")
;;     ;; (org-id-get-create)
;;     ;; (save-buffer)
;;     ;; (kill-buffer nil)
;;     )
;;   )


;; (defun org-org-template (contents info)
;;   "Return Org document template with document keywords.
;; CONTENTS is the transcoded contents string.  INFO is a plist used
;; as a communication channel."
;;   (concat
;;    (and (plist-get info :time-stamp-file)
;; 	      (format-time-string "# Created %Y-%m-%d %a %H:%M\n"))
;;    (org-element-normalize-string
;;     (mapconcat #'identity
;; 	             (org-element-map (plist-get info :parse-tree) 'keyword
;; 		             (lambda (k)
;; 		               (and (string-equal (org-element-property :key k) "OPTIONS")
;; 			                  (concat "#+options: "
;; 				                        (org-element-property :value k)))))
;; 	             "\n"))
;;    (and (plist-get info :with-title)
;; 	      (format "#+title: %s\n" (org-export-data (plist-get info :title) info)))
;;    (and (plist-get info :with-date)
;; 	      (let ((date (org-export-data (org-export-get-date info) info)))
;; 	        (and (org-string-nw-p date)
;; 	             (format "#+date: %s\n" date)
;;                )))
;;    (and (plist-get info :with-date)
;; 	      (let ((identifier (org-export-data (org-export-get-date info) info)))
;; 	        (and (org-string-nw-p identifier)
;; 	             (format "#+identifier: %s\n" (my-denote-format-time identifier))
;;                )))
;;    (and (plist-get info :with-author)
;; 	      (let ((author (org-export-data (plist-get info :author) info)))
;; 	        (and (org-string-nw-p author)
;; 	             (format "#+author: %s\n" author))))
;;    (and (plist-get info :with-email)
;; 	      (let ((email (org-export-data (plist-get info :email) info)))
;; 	        (and (org-string-nw-p email)
;; 	             (format "#+email: %s\n" email))))
;;    (and (plist-get info :with-creator)
;; 	      (org-string-nw-p (plist-get info :creator))
;; 	      (format "#+creator: %s\n" (plist-get info :creator)))
;;    contents))


;; (defun azr/org-roam-modify-title ()
;;     "Modify title of org-roam current node and update all backlinks in roam database."
;;     (interactive)
;;     (unless (org-roam-buffer-p) (error "Not in an org-roam buffer."))
;;     (save-some-buffers t)
;;     (let* ((old-title (org-roam-get-keyword "title"))
;;            (ID (org-entry-get (point) "ID"))
;;            (new-title (read-string "Enter new title: " old-title)))
;;       (org-roam-set-keyword "title" new-title)
;;       (save-buffer)
;;       (let* ((new-slug (org-roam-node-slug (org-roam-node-at-point)))
;;              (new-file-name (replace-regexp-in-string "-.*\\.org" (format "-%s.org" new-slug) (buffer-file-name)))
;;              (new-buffer-name (file-name-nondirectory new-file-name)))
;; 	(rename-buffer new-buffer-name)
;; 	(rename-file (buffer-file-name) new-file-name 1)
;; 	(set-visited-file-name new-file-name)) ; I don't know why this last command is necessary. Getting it from here: https://stackoverflow.com/a/384346/2422698
;;       (save-buffer)
;;       ;; Rename backlinks in the rest of the Org-roam database.
;;       (let* ((search (format "[[id:%s][%s]]" ID old-title))
;;              (replace (format "[[id:%s][%s]]" ID new-title))
;;              (rg-command (format "rg -t org -lF %s ~/Org/roam/" search))
;;              (file-list (split-string (shell-command-to-string rg-command))))
;; 	(dolist (file file-list)
;;           (let ((file-open (get-file-buffer file)))
;; 	    (find-file file)
;;             (beginning-of-buffer)
;;             (while (search-forward search nil t)
;;               (replace-match replace))
;;             (save-buffer)
;;             (unless file-open
;;               (kill-buffer)))))))

;; (org-capture-string (format "%s %s :%s:\n\n%s %s %s :%s:\n%s"
;;                             (s-repeat (org-element-property :level from-project) "*")
;;                             (org-element-property :raw-value from-project)
;; (s-join ":" (org-element-property :tags from-project))
;;           (setq random-line (string-join (split-string random-line) " ")))

;;; funcs.el ends here

```


#### <span class="section-num">3.10.4</span> The `jh-org` keybindings.el {#h:94d5c63f-a3b2-4271-9768-3585fe9ddb7a}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;; ~/sync/org/roam/configs/jh-org.org

;;; Code:

;;; Function Key F1-F12

(global-set-key (kbd "C-c l") 'org-store-link)
(global-set-key (kbd "C-c i") 'org-insert-link)
(global-set-key (kbd "C-c a") 'org-agenda)
(global-set-key (kbd "C-c c") 'org-capture)
(global-set-key (kbd "C-c N") 'consult-notes)
(global-set-key (kbd "C-c \\") 'org-tags-sparse-tree)

(global-set-key (kbd "C-c C-x C") 'org-clone-subtree-with-time-shift)
(define-key org-mode-map (kbd "C-c C-x C") 'org-clone-subtree-with-time-shift)

(global-set-key (kbd "<f9> <f9>") 'bh/show-org-agenda)

;; (global-set-key (kbd "<f9> b") 'bbdb)
;; (global-set-key (kbd "<f9> g") 'gnus)
;; @TODO: Remove `bh/hide-other' in the future if `outline-hide-other'
;; proves to be better.
;; (global-set-key (kbd "<f9> h") 'bh/hide-other)
(global-set-key (kbd "<f9> h") 'outline-hide-other)

(global-set-key (kbd "<f9> k") 'my/get-id-to-clipboard)
(global-set-key (kbd "<f9> r") 'org-remark-mark-line)

;; with markdown-mode-map
(define-key org-mode-map (kbd "<f9> l") 'org-toggle-link-display)
(define-key org-mode-map (kbd "<f9> g") 'org-toggle-inline-images)
(define-key org-mode-map (kbd "<f9> m") 'my/org-toggle-emphasis-markers)

(global-set-key (kbd "<f9> S") 'gif-screencast-start-or-stop)
(global-set-key (kbd "<f9> f") 'my/logos-focus-editing-toggle)

(global-set-key (kbd "<f9> i") 'org-clock-in)

;;; global

(global-set-key (kbd "C-c R") 'org-randomnote)

;; (global-set-key (kbd "M-s /") #'consult-org-roam-file-find)

(global-set-key (kbd "C-c C-a") #'my/consult-org-agenda-heading)

(global-set-key (kbd "C-x x v") 'my/view-text-file-as-info-manual)

;; 파일 패스 복사 =SPC f y l=
;; (global-set-key (kbd "C-c f") 'spacemacs/copy-file-path)
(global-set-key (kbd "C-c f") 'spacemacs/projectile-copy-file-path)
(global-set-key (kbd "C-c F") 'spacemacs/copy-file-path-with-line)

;; (global-set-key (kbd "C-c j o r") 'open-org-refile-file)
(global-set-key (kbd "C-c j o n") 'org-notes-files)
(global-set-key (kbd "C-c j o w") 'org-workflow-files)
(global-set-key (kbd "C-c j o S") 'org-gcal-sync)
(global-set-key (kbd "C-c j o t") 'org-make-toc)

(global-set-key (kbd "C-x n r") 'narrow-to-region)

(global-set-key (kbd "M-c f") 'org-fc-hydra/body)

;;;; org-remark

;; Key-bind `org-remark-mark' to global-map so that you can call it
;; globally before the library is loaded.

(define-prefix-command 'org-remark-map)
(define-key global-map (kbd "C-c r") 'org-remark-map)
(let ((map org-remark-map))
  ;; The rest of keybidings are done only on loading `org-remark'
  (define-key map (kbd "m") #'org-remark-mark)
  (define-key map (kbd "l") #'org-remark-mark-line)
  (define-key map (kbd "k") #'org-remark-change))

(with-eval-after-load 'org-remark
  (define-key org-remark-mode-map (kbd "C-c r o") #'org-remark-open)
  (define-key org-remark-mode-map (kbd "C-c r r") #'org-remark-remove)
  (define-key org-remark-mode-map (kbd "C-c r d") #'org-remark-delete)
  (define-key org-remark-mode-map (kbd "C-c r n") #'org-remark-view-next)
  (define-key org-remark-mode-map (kbd "C-c r p") #'org-remark-view-prev)
  ;; (define-key org-remark-mode-map (kbd "C-c r ]") #'org-remark-view-next)
  ;; (define-key org-remark-mode-map (kbd "C-c r [") #'org-remark-view-prev)
  )

;;;; side-notes

(global-set-key (kbd "M-g s") 'side-notes-toggle-notes)

;;;; yankpad

(global-set-key (kbd "M-g y") 'yankpad-insert)

;;; evil org-mode-map

(with-eval-after-load 'org
  ;; from DW
  (evil-define-key '(normal insert visual) org-mode-map (kbd "C-n") 'org-next-visible-heading)
  (evil-define-key '(normal insert visual) org-mode-map (kbd "C-p") 'org-previous-visible-heading)

  ;; (evil-define-key '(insert) org-mode-map (kbd "M-h") 'delete-backward-char)
  ;; (evil-define-key '(insert) org-mode-map (kbd "M-l") 'delete-forward-char)

  ;; ordered/unordered list 를 입력 할 때 편함.
  ;; 체크박스가 있는 경우 M-S-RET org-insert-todo-heading 을 활용.
  ;; (evil-define-key '(normal insert visual) org-mode-map (kbd "C-M-<return>") 'org-insert-item)

  ;; 문단을 한 라인으로 합쳐 준다. 구글 번역기 돌릴 때 매우 유용.
  ;; (evil-define-key '(normal insert visual) org-mode-map (kbd "C-M-q") 'unfill-paragraph)

  ;; 복사한 링크는 아래의 방법으로 넣는다. 깔끔해서 좋다.
  ;; org-cliplink 는 insert 니까 i 를 바인딩한다. org-insert-link 를 따른다.
  (evil-define-key '(normal insert visual) org-mode-map (kbd "C-c M-i") 'org-cliplink)
  ;; (define-key map (kbd "C-c M-i") 'org-cliplink)

  (evil-define-key '(insert) org-mode-map (kbd "C-u") 'undo-fu-only-undo)
  (evil-define-key '(insert) org-mode-map (kbd "C-r") 'undo-fu-only-redo)

  ;; (evil-define-key '(insert) org-mode-map (kbd "C-0") 'begginng-of-line)
  ;; (evil-define-key '(insert) org-mode-map (kbd "C-4") 'end-of-line)

  ;; flameshot 으로 스크린샷 한 뒤, 바로 붙여넣기
  ;; 22/10/04--15:18 :: flameshot 저장하면 자동으로 클립보드에
  ;; full-path 가 복사된다. imglink 스니펫을 부르고 경로를 복사한다.
  ;; 스크린샷 및 이미지를 관리하기에 이러한 방법이 더 좋다.
  (evil-define-key '(normal insert visual) org-mode-map (kbd "C-c M-y") 'org-download-clipboard)
  ;; (define-key map (kbd "C-c M-y") 'org-download-clipboard)
  )

;;; org-mode-map

(define-key org-mode-map (kbd "C-c C-x n") #'ews-org-insert-notes-drawer)

;; search to narrow with heading and tag
(define-key org-mode-map (kbd "C-c o") 'consult-org-heading)
(define-key org-mode-map (kbd "C-M-i") 'completion-at-point)

(define-key prog-mode-map (kbd "C-M-y") 'evil-yank)
(define-key org-mode-map (kbd "C-M-y") 'org-rich-yank)

(define-key org-mode-map (kbd "C-c y") 'org-cliplink)
(define-key org-mode-map (kbd "C-c I") 'org-insert-link-dwim) ; org-insert-link

;; Shortcuts to Interactive Functions
;; "C-x n" prefix
(global-set-key (kbd "C-x n m") #'my/split-and-indirect-orgtree)
(global-set-key (kbd "C-x n M") #'my/kill-and-unsplit-orgtree)

(spacemacs/set-leader-keys "nm" #'my/split-and-indirect-orgtree)
(spacemacs/set-leader-keys "nM" #'my/kill-and-unsplit-orgtree)
(spacemacs/set-leader-keys "tM" #'my/org-toggle-emphasis-markers)

(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "CP" 'ash/org-pomodoro-til-meeting)
(spacemacs/set-leader-keys-for-major-mode 'org-agenda-mode
  "CP" 'ash/org-pomodoro-til-meeting) ;; "Start pomodoro til half hour"

(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "E" 'org-set-effort
  "D" 'org-deadline
  "S" 'org-schedule
  "g" 'consult-org-heading
  "\\" 'org-tags-sparse-tree
  "TI" 'org-imgtog-mode
  "Tm" 'org-toggle-item
  "TM" 'my/org-toggle-emphasis-markers
  "Th" 'org-toggle-heading
  "Tp" 'org-tree-slide-mode
  "TP" 'org-present
  "i1" 'my/org-get-heading-title
  "i2" 'my/org-insert-heading-category
  "i3" 'my/org-insert-heading-categories-all
  "R" 'org-delete-link
  "F" 'org-insert-file-link
  "I" 'org-insert-link-dwim

  "mm" 'er/mark-paragraph
  )

;;; ctl-x-x-map

(define-key ctl-x-x-map "h" #'prot-org-id-headline) ; C-x x h
(define-key ctl-x-x-map "H" #'prot-org-id-headlines)
(define-key ctl-x-x-map "e" #'prot-org-ox-html)

;;; spacemacs key

;;;; global 'oc'

(spacemacs/set-leader-keys "oc" nil)
(spacemacs/declare-prefix "oc"  "my/functions")
(spacemacs/set-leader-keys
  "occ" 'my/genfile-timestamp
  "ocd" 'my/get-file-line
  "oce" 'my/get-file-link
  "ocf" 'my/encode-buffer-to-utf8
  "ocg" 'my/copy-word
  "och" 'my/copy-line
  "oci" 'my/copy-paragraph
  "ocj" 'my/copy-buffer
  "ock" 'my/backward-last-edit
  "ocl" 'my/buffer-edit-hook
  "oct" 'my/org-titlecase-level-1
  "ocm" 'my/rename-file-and-buffer
  "ocn" 'my/grep-find
  "oco" 'my/open-external
  "ocp" 'my/open-external-pdf
  "ocq" 'my/unfill-paragraph-or-region
  ;; 'my/unfill-paragraph-keep-formatting
  ;; 'my/unfill-region-smart
  )

;;;; glocal 'Cw'

(spacemacs/set-leader-keys "Ca" #'orgabilize-org-archive) ; url to org
(spacemacs/set-leader-keys "Cl" #'orgabilize-insert-org-link)
(spacemacs/set-leader-keys "Cf" #'orgabilize-org-find-archived-file) ; html to org
(spacemacs/set-leader-keys "CA" #'orgabilize-org-archive-from-file)

(spacemacs/set-leader-keys "CA" #'orgabilize-org-archive-from-file)
(spacemacs/set-leader-keys "Cw" #'wikinforg)

;;;; org-mode 'm'

(spacemacs/declare-prefix-for-mode 'org-mode "mo" "custom")
(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "oc" 'my/genfile-timestamp
  "od" 'my/get-file-line
  "oe" 'my/get-file-link
  "of" 'my/encode-buffer-to-utf8
  "og" 'my/copy-word
  "oh" 'my/copy-line
  "oi" 'my/copy-paragraph
  "oj" 'my/copy-buffer
  "ok" 'my/backward-last-edit
  "ot" 'my/org-titlecase-level-1
  "ol" 'my/buffer-edit-hook
  "om" 'my/rename-file-and-buffer
  "on" 'my/grep-find
  "oo" 'my/open-external
  "op" 'my/open-external-pdf
  "oq" 'my/unfill-paragraph-or-region

  "i0" 'cc-todo-item
  "mi" 'org-id-get-create
  )

;;;; Notes - org-roam and legacy

;; "o" 'link-hint-open-link
;; "r" 'org-roam-buffer-refresh
;; "nk" #'org-id-get-create

;; 'C-c n' my-org-notes-map
(define-prefix-command 'notes-map)
(define-key global-map (kbd "C-c n") 'notes-map)
(let ((map notes-map))
  (define-key map (kbd "b") #'citar-create-note)
  (define-key map (kbd "n") #'consult-notes) ;; consult-notes
  (define-key map (kbd "f") #'org-roam-node-find) ;; consult-notes
  (define-key map (kbd "s") #'org-roam-db-sync)
  ;; (define-key map (kbd "S") #'org-roam-headline-db-sync)
  (define-key map (kbd "k") #'org-roam-create-id-sync-db)
  (define-key map (kbd "h") #'consult-org-roam-headline)
  )

(spacemacs/declare-prefix-for-mode 'org-mode "mn" "notes")

(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "nb" 'citar-create-note
  "nf" 'org-roam-node-find
  "nk" 'org-roam-create-id-sync-db
  "np" 'org-project-capture-project-todo-completing-read
  "ns" 'org-roam-db-sync
  "n/" 'org-roam-node-random
  )

;;;; Denote : C-c w and mw

(global-set-key (kbd "C-c w d") #'open-dired-denote-directory)
(global-set-key (kbd "C-c w n") #'consult-notes)
(global-set-key (kbd "C-c w N") #'consult-notes-search-in-all-notes)
(global-set-key (kbd "C-c w e") #'prot-eshell-export)
(global-set-key (kbd "C-c w C-e") #'efls/denote-signature-buffer)
;; (define-key eshell-mode-map (kbd "C-c C-e") #'prot-eshell-export) ; eshell-show-maximum-output

(define-prefix-command 'citar-denote-map)
(define-key global-map (kbd "C-c w c") 'citar-denote-map)
(let ((map citar-denote-map))
  (define-key map (kbd "c") 'citar-create-note)
  (define-key map (kbd "i") 'citar-insert-citation)
  (define-key map (kbd "r") 'citar-insert-reference)

  (define-key map (kbd "n") 'citar-denote-open-note)
  (define-key map (kbd "d") 'citar-denote-dwim)
  (define-key map (kbd "e") 'citar-denote-open-reference-entry)
  (define-key map (kbd "a") 'citar-denote-add-citekey)
  (define-key map (kbd "k") 'citar-denote-remove-citekey)
  (define-key map (kbd "f") 'citar-denote-find-citation)
  (define-key map (kbd "F") 'citar-denote-find-reference)
  (define-key map (kbd "l") 'citar-denote-link-reference))

(spacemacs/declare-prefix-for-mode 'org-mode "mw" "Denote")
(spacemacs/declare-prefix-for-mode 'org-mode "mwf" "find")
(spacemacs/declare-prefix-for-mode 'org-mode "mwc" "citar")
(spacemacs/declare-prefix-for-mode 'org-mode "mwd" "dired")
(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "wn" 'denote
  "wR" 'denote-region
  "wN" 'denote-type
  "wD" 'denote-date
  "wz" 'denote-signature
  "ws" 'denote-subdirectory
  "wt" 'denote-template
  "wl" 'denote-link
  "wL" 'denote-add-links
  "wb" 'denote-backlinks
  "wk" 'denote-keywords-add
  "wK" 'denote-keywords-remove

  "wi" 'denote-org-dblock-insert-links
  "wI" 'denote-org-dblock-insert-backlinks

  "wff" 'denote-find-link
  "wfb" 'denote-find-backlink

  "wr" 'denote-region
  "wR" 'denote-rename-file-using-front-matter
  "w M-r" 'denote-rename-file

  "wcc" 'citar-create-note
  "wcn" 'citar-denote-open-note
  "wcd" 'citar-denote-dwim
  "wci" 'citar-insert-citation
  "wcr" 'citar-insert-reference
  "wce" 'citar-denote-open-reference-entry
  "wca" 'citar-denote-add-citekey
  "wck" 'citar-denote-remove-citekey
  "wcf" 'citar-denote-find-citation
  "wcF" 'citar-denote-find-reference
  "wcl" 'citar-denote-link-reference
  )

;;;;; dired and denote

(spacemacs/declare-prefix-for-mode 'dired-mode "mw" "denote")
(spacemacs/declare-prefix-for-mode 'dired-mode "mwf" "find")
(spacemacs/declare-prefix-for-mode 'dired-mode "mwc" "citar")
(spacemacs/declare-prefix-for-mode 'dired-mode "mwd" "dired")
(spacemacs/set-leader-keys-for-major-mode 'dired-mode
  "wn" 'denote
  "wN" 'denote-type
  "wD" 'denote-date
  "wz" 'denote-signature
  "ws" 'denote-subdirectory
  "wt" 'denote-template
  "wl" 'denote-link
  "wL" 'denote-add-links
  "wb" 'denote-backlinks
  "wk" 'denote-keywords-add
  "wK" 'denote-keywords-remove

  ;; "wi" 'denote-org-dblock-insert-links
  ;; "wI" 'denote-org-dblock-insert-backlinks

  "wff" 'denote-find-link
  "wfb" 'denote-find-backlink

  "wr" 'denote-region
  "wR" 'denote-rename-file-using-front-matter
  "w M-r" 'denote-rename-file

  "wcc" 'citar-create-note
  "wcn" 'citar-denote-open-note
  "wcd" 'citar-denote-dwim
  "wce" 'citar-denote-open-reference-entry
  "wca" 'citar-denote-add-citekey
  "wck" 'citar-denote-remove-citekey
  "wcr" 'citar-denote-find-reference
  "wcf" 'citar-denote-find-citation
  "wcl" 'citar-denote-link-reference

  "wdi" 'denote-link-dired-marked-notes
  "wdr" 'denote-dired-rename-files
  "wds" 'denote-sort-dired
  "wdk" 'denote-dired-rename-marked-files-with-keywords
  "wdR" 'denote-dired-rename-marked-files-using-front-matter
  )


;;;; DONT tmr and timer

;; (define-prefix-command 'tmr-map)
;; (define-key global-map (kbd "C-c t") 'tmr-map)

;; (let ((map tmr-map))
;;   (define-key map (kbd "t") #'tmr)
;;   (define-key map (kbd "T") #'tmr-with-description)
;;   (define-key map (kbd "l") #'tmr-tabulated-view) ; "list timers" mnemonic
;;   (define-key map (kbd "c") #'tmr-clone)
;;   (define-key map (kbd "k") #'tmr-cancel)
;;   (define-key map (kbd "s") #'tmr-reschedule)
;;   (define-key map (kbd "e") #'tmr-edit-description)
;;   (define-key map (kbd "r") #'tmr-remove)
;;   (define-key map (kbd "R") #'tmr-remove-finished)

;;   ;; 'C-c t' work with tmr
;;   ;; (define-key map (kbd "d") 'gts-do-translate)
;;   )

;;; packages.el ends here

```


### <span class="section-num">3.11</span> The `jh-misc` layer {#h:f0f80679-c399-4ee5-8ea7-6ce46a33eaed}




#### <span class="section-num">3.11.1</span> The `jh-misc` layer.el {#h:8ad99ce3-e05c-4156-9b56-7d3042af9185}



```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
;;
;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
;;
;; Author: Junghan Kim <junghanacs@gmail.com>
;; URL: https://github.com/junghanacs
;;
;; This file is not part of GNU Emacs.
;;
;; License: GPLv3

;;; Commentary:

;;; Code:

(configuration-layer/declare-layer-dependencies
 '(
   ;; devdocs-browser 이용. 한글 문서
   (spacemacs-misc :packages (not dumb-jump devdocs)) ; request

   games

   quickurl

   ;; openai

   (finance
    :variable
    ;; ledger-clear-whole-transactions t
    ;; ledger-report-use-native-highlighting nil
    ;; ledger-accounts-file (expand-file-name "~/personal/finances/data/accounts.dat")
    ledger-post-amount-alignment-column 68
    )

;;;; Tools

   ;; (geolocation :variables
   ;;              geolocation-enable-automatic-theme-changer nil
   ;;              geolocation-enable-location-service t
   ;;              geolocation-enable-weather-forecast t
   ;;              sunshine-show-icons t
   ;;              sunshine-units 'metric
   ;;              sunshine-location user-calendar-location-name
   ;;              sunshine-appid user-sunshine-appid
   ;;              )

   ;; ansible
   ;; terraform
   ;; vagrant
   ;; pass

;;;; Music

   ;; pianobar
   ;; tidalcycles ; live coding

;;;; 24. Web services

   search-engine
   ;; (search-engine :variables
   ;;                search-engine-config-list '((custom1
   ;;                                             :name "Custom Search Engine 1"
   ;;                                             :url "http://www.domain.com/s/stuff_sutff_remember_to_replace_search_candidate_with_%s"
   ;;                                             :keywords (:docstring "My custom string"
   ;;                                                                   :browser 'eww-browse-url))))

   ;; eaf
   )
 )

```


#### <span class="section-num">3.11.2</span> The `jh-misc` packages.el {#h:f270ca5a-dd66-40cd-b6ce-3681b3dfbcdc}

<!--list-separator-->

1.  Packages



    ```elisp
    ;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-
    ;;
    ;; Copyright (c) 2012-2023 Sylvain Benner & Contributors
    ;;
    ;; Author: Junghan Kim <junghanacs@gmail.com>
    ;; URL: https://github.com/junghanacs
    ;;
    ;; This file is not part of GNU Emacs.
    ;;
    ;; License: GPLv3

    ;;; Commentary:

    ;;; Code:

    ;;;; Package Lists

    (defconst jh-misc-packages
      '(
        wakatime-mode
        ;; activity-watch-mode

        keyfreq
        keycast
        interaction-log
        ;; (command-log :location (recipe :fetcher github
        ;;                                :repo "positron-solutions/command-log"))

        (browser-hist :location (recipe :fetcher github
                                        :repo "agzam/browser-hist.el"))

        frameshot
        gif-screencast
        ;; (screenshot :location (recipe :fetcher github :repo "tecosaur/screenshot"
        ;;                               :files ("*.el" "*.org")))

        atomic-chrome
        redacted

        disk-usage
        ;;  biome

        ;; elcord
        ;; telega
        ;; ement
        ))

    ```

<!--list-separator-->

2.  Configurations

    ```elisp


    ;;;; atomic-chrome

    (defun jh-misc/init-atomic-chrome ()
      (use-package atomic-chrome
        :if (not (or my/remote-server *is-termux*))
        :defer 10
        :commands (atomic-chrome-start-server)
        :hook (after-init . atomic-chrome-start-server)
        ))

    ;;;; redacted : obsecure buffer

    (defun jh-misc/init-redacted ()
      (use-package redacted
        :if (not (or my/remote-server *is-termux*))
        :defer (spacemacs/defer)
        :commands (redacted-mode))
      )

    ;;;; Screencast

    (defun jh-misc/init-gif-screencast ()
      (require 'gif-screencast))

    (defun jh-misc/init-frameshot ()
      (use-package frameshot
        :if (not (or my/remote-server *is-termux*))
        :defer 10)
      )

    ;; conflict with winner-mode
    ;; This makes it a breeze to take lovely screenshots.
    ;; (defun jh-misc/init-screenshot ()
    ;;   (use-package screenshot :defer 10)
    ;;   )

    ;;;;; Keycast

    (defun jh-misc/init-keycast ()
      (use-package keycast
        :ensure
        :config
        ;; (setq keycast-tab-bar-minimal-width 25) ; 2023-07-02 30 -> 25
        (setq keycast-tab-bar-minimal-width 50)
        (setq keycast-tab-bar-format "%10s%k%c%r")

        ;; (unless *is-termux*
        ;;   (add-hook 'spacemacs-post-user-config-hook 'keycast-tab-bar-mode))

        (when (string= (system-name)"jhnuc")
          (add-hook 'spacemacs-post-user-config-hook 'keycast-tab-bar-mode))

        (dolist (input '(self-insert-command
                         org-self-insert-command))
          (add-to-list 'keycast-substitute-alist `(,input ">>>>>>>>" "Typing.....")))
        ;; (add-to-list 'keycast-substitute-alist `(,input "." "Typing…")))

        (dolist (event '(mouse-event-p
                         mouse-movement-p
                         mwheel-scroll

                         ;; 2023-10-02 Added for clojure-dev
                         lsp-ui-doc--handle-mouse-movement
                         ignore-preserving-kill-region
                         ;; mouse-set-region
                         ;; mouse-set-point
                         ))
          (add-to-list 'keycast-substitute-alist `(,event nil)))
        )
      )


    ;;;; Interaction-log

    ;; Interaction Log is like =view-lossage= (=C-h l=) or =kmacro-edit-macro= but
    ;; it is live-updating and not tied to macros. It's useful for when you type an
    ;; (awesome? terrible?) Emacs command and want to figure out which function you
    ;; used so you can use it again or destroy it forever. For a long time I was
    ;; plagued by accidentally hitting =downcase-region= and didn't know what the
    ;; function was - this would have been so useful!

    ;; 인터랙션 로그는 =view-lossage= (=C-h l=) 또는 =kmacro-edit-macro=와 비슷하지만,
    ;; 매크로에 묶여 있지 않고 실시간으로 업데이트됩니다. (멋진? 끔찍한?) Emacs 명령을
    ;; 입력한 후 어떤 기능을 사용했는지 파악하여 다시 사용하거나 영원히 파괴하고 싶을
    ;; 때 유용합니다. 오랫동안 실수로 =downcase-region=을 눌러서 어떤 함수인지 몰라
    ;; 골머리를 앓았는데, 이 기능이 있었다면 정말 유용했을 거예요!

    (defun jh-misc/init-interaction-log ()
      (require 'interaction-log)
      ;; (unless *is-termux*
      ;;   (interaction-log-mode +1))
      )

    ;;;; command-log

    ;; (defun jh-misc/init-command-log ()
    ;;   (use-package command-log
    ;;     :if (not (or my/remote-server *is-termux*))
    ;;     :defer 5
    ;;     :init
    ;;     (spacemacs/declare-prefix "atl" "command log")
    ;;     (spacemacs/set-leader-keys "atll" #'global-command-log-mode)
    ;;     :custom
    ;;     (command-log-logging-shows-buffer t "Toggling will show the buffer.")
    ;;     (command-log-window-text-scale 0 "Command log two steps higher text scale")
    ;;     (command-log-hiding-disables-logging t "Toggling visible buffer turns off logging.")
    ;;     (command-log-disabling-logging-kills-buffer t "The buffer will be new when displayed again.")
    ;;     (command-log-filter-commands '(self-insert-command) "Be chatty.
    ;;      Show everything besides self-insert-command")
    ;;     ;; Auto-enable with global minor mode (including minibuffer)
    ;;     (command-log-log-globally t)
    ;;     )
    ;;   )

    ;;;; keyfreq

    (defun jh-misc/init-keyfreq()
      (use-package keyfreq
        :if (not (or my/remote-server *is-termux*))
        :defer 5
        :config
        (keyfreq-mode 1)
        (keyfreq-autosave-mode 1)
        (setq keyfreq-excluded-commands
              '(self-insert-command
                forward-char
                evil-forward-char
                backward-char
                evil-backward-char
                previous-line
                next-line)))
      )

    ;;;; disk-usage

    (defun jh-misc/init-disk-usage ()
      (use-package disk-usage :after evil-collection :defer 20)
      )

    ;;;; browser-hist

    (defun jh-misc/init-browser-hist ()
      (use-package browser-hist
        :if (not (or my/remote-server *is-termux*))
        :init
        (require 'embark)
        :commands (browser-hist-search)
        :config
        (require 'sqlite)
        ;; (require 'embark) ; load Embark before the command (if you're using it)
        (setq browser-hist-db-paths
              '((chrome . "$HOME/.config/google-chrome/Default/History")
                (brave . "$HOME/.config/BraveSoftware/Brave-Browser/Default/History")
                (whale . "$HOME/.config/naver-whale/Default/History")
                (firefox . "$HOME/.mozilla/firefox/*.default-release-*/places.sqlite") ; 9z6k8asp.default-release
                (chromium . "$HOME/.config/Chromium/Default/History")))
        (setq browser-hist-default-browser 'firefox) ; whale
        )
      )

    ;;;; activity-watch

    ;; (defun jh-misc/init-activity-watch-mode ()
    ;;   (use-package activity-watch-mode
    ;;     :defer 10
    ;;     :config
    ;;     (defun spacemacs/activitywatch-dashboard ()
    ;;       (interactive)
    ;;       (browse-url "http://localhost:5600"))
    ;;     ;; (global-activity-watch-mode 1)
    ;;     ))

    ;;;; wakatime

    ;; $ python3 -c "$(wget -q -O - https://raw.githubusercontent.com/wakatime/vim-wakatime/master/scripts/install_cli.py)"
    (defun jh-misc/init-wakatime-mode ()
      (use-package wakatime-mode
        :if (and (or
                  (string= (system-name) "jhnuc")
                  (string= (system-name) "junghan-laptop")
                  )
                 (not my/slow-ssh)
                 (not my/remote-server))
        :init
        (add-hook 'prog-mode-hook 'wakatime-mode)
        (add-hook 'org-mode-hook 'wakatime-mode)
        (add-hook 'markdown-mode-hook 'wakatime-mode)
        :defer 5
        :config
        (advice-add 'wakatime-init :after (lambda () (setq wakatime-cli-path (expand-file-name "~/.wakatime/wakatime-cli"))))

        ;; wakatime-api-key  "your-api-key" in permachine.el
        (defun spacemacs/wakatime-dashboard ()
          (interactive)
          (browse-url "https://wakatime.com/dashboard"))
        (global-wakatime-mode)
        ))

    ;;;; elcord

    ;; (defun jh-misc/init-elcord ()
    ;;   (use-package elcord
    ;;    :if (and (or
    ;;           (string= (system-name) "jhnuc")
    ;;           (string= (system-name) "junghan-laptop"))
    ;;          (not my/slow-ssh)
    ;;          (not my/remote-server))
    ;;    :defer (spacemacs/defer)
    ;;    :config
    ;;    (setq elcord-client-id user-elcord-client-id) ;; APP ID
    ;;    ;; (setq elcord-buffer-details-format-function #'my/elcord-buffer-details-format-functions)
    ;;    ;; (advice-add 'elcord--try-update-presence :filter-args #'my/elcord-update-presence-mask-advice)
    ;;    ;; (add-to-list 'elcord-mode-text-alist '(telega-chat-mode . "Telega Chat"))
    ;;    ;; (add-to-list 'elcord-mode-text-alist '(telega-root-mode . "Telega Root"))
    ;;    ;; (elcord-mode)
    ;;    ))

    ;;;; ement
    ;; JunghanKim : @junghan0611:gitter.im

    ;; (defun jh-misc/init-ement ()
    ;;   (use-package ement :defer 10)
    ;;   )

    ;;;; literate-calc-mode

    ;; (defun jh-misc/init-literate-calc-mode ()
    ;;   (use-package literate-calc-mode
    ;;     :commands literate-calc-minor-mode)
    ;;   )

    ;;;; biome : open-meteo client

    ;; [[https://open-meteo.com/][open-meteo]] client.
    ;; (use-package biome
    ;;  :commands (biome)
    ;;  ;; :init
    ;;  ;; (my-leader-def "ab" #'biome)
    ;;  :config
    ;;  (add-to-list 'biome-query-coords
    ;;           '("Suwon, Korea" 37.2911 127.0089)) ; Suwon
    ;;  ;; (add-to-list 'biome-query-coords
    ;;  ;;              '("Tyumen, Russia" 57.15222 65.52722))
    ;;  )

    ;;; packages.el ends here

    ```


#### <span class="section-num">3.11.3</span> The `jh-misc` funcs.el {#h:b8f3d308-3448-412e-a1f1-8d7647d870c8}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; SVG screenshots
;;----------------------------------------------------------------------
;; https://github.com/mwh/dragon make NAME=dragon-drop install
;; (defun screenshot-svg ()
;;   "Save a screenshot of the current frame as an SVG image.
;; Puts the filename in the kill ring and displays it using an
;; X-sink (Executable dragon-drag-and-drop if available)."
;;   (interactive)
;;   (let ((filename (concat (file-name-as-directory
;;                             (concat (getenv "HOME") "/Pictures/screenshots"))
;;                     (format "%s_%sx%s_emacs.svg"
;;                       (format-time-string "%Y-%m-%d-%H%M%S")
;;                       (frame-pixel-width)
;;                       (frame-pixel-height))))
;;          (data (x-export-frames nil 'svg)))
;;     (with-temp-file filename
;;       (insert data))
;;     (kill-new filename)
;;     (if (executable-find "dragon-drag-and-drop")
;;       (start-process "drag_screenshot_svg" "drag_screenshot_svg"
;;         "dragon-drag-and-drop" filename)
;;       (message "Could not find dragon-drag-and-drop"))
;;     (message filename)))
;; (global-set-key (kbd "<C-print>") #'screenshot-svg) ; Ctrl + fn1 + p

(defun my/screenshot-svg ()
  "Save a screenshot of the current frame as an SVG image.
  Saves to a temp file and puts the filename in the kill ring."

  ;; Taken from here
  ;; https://www.reddit.com/r/emacs/comments/idz35e/emacs_27_can_take_svg_screenshots_of_itself/g2c2c6y/
  (interactive)
  (let* ((filename (make-temp-file "Emacs" nil ".svg"))
         (data (x-export-frames nil 'svg)))
    (with-temp-file filename
      (insert data))
    (kill-new filename)
    (message filename)))

(defun serve-current-buffer (&optional port)
  "Serve current buffer."
  (interactive)
  (ws-start
   (lambda (request)
     (with-slots (process headers) request
       (ws-response-header process 200 '("Content-type" . "text/html; charset=utf-8"))
       (process-send-string process
                            (let ((html-buffer (htmlize-buffer)))
                              (prog1 (with-current-buffer html-buffer (buffer-string))
                                (kill-buffer html-buffer))))))
   (or port 9010)))

(defcustom engine-mode/github-mode->lang
  '(("clojurescript" . "Clojure")
    ("clojure" . "Clojure")
    ("clojurec" . "Clojure")
    ("emacs-lisp" . "Emacs Lisp")
    ("Info" . "Emacs Lisp")
    ("lisp-data" . "Emacs Lisp")
    ("helpful" . "Emacs Lisp")
    ("js" . "JavaScript"))
  "Associates current mode with a language in Github terms"
  :type 'alist
  :group 'engine)

;;;###autoload
(defun engine/search-github-with-lang ()
  "Search on Github with attempt of detecting language associated with current-buffer's mode"
  (interactive)
  (let* ((mode-name (replace-regexp-in-string "-mode$" "" (symbol-name major-mode)))
         (lang (cdr (assoc mode-name engine-mode/github-mode->lang)))
         (lang-term (if lang (concat "language:\"" lang "\" ") ""))
         (current-word (if (region-active-p)
                           (buffer-substring (region-beginning) (region-end))
                         (thing-at-point 'symbol)))
         (search-term (read-string "Search Github: " (concat lang-term current-word))))
    (engine/search-github search-term)))
```


#### <span class="section-num">3.11.4</span> The `jh-misc` keybindings.el {#h:5ce62b8d-7809-47ef-b9db-2d494e0c15b8}

```elisp

;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; (spacemacs/set-leader-keys "aW" 'spacemacs/wakatime-dashboard)
(spacemacs/set-leader-keys "aA" 'spacemacs/activitywatch-dashboard)

(spacemacs/set-leader-keys "Tr" 'redacted-mode)
(global-set-key (kbd "M-c M-l") 'redacted-mode)

;; 신기할세. 붙이면 안되네?!
(spacemacs/set-leader-keys "s w s" 'engine/search-stack-overflow)
(spacemacs/set-leader-keys "s w e" 'engine/search-ecosia)
(spacemacs/set-leader-keys "s w d" 'engine/search-duck-duck-go)
(spacemacs/set-leader-keys "s w g" 'engine/search-github)
(spacemacs/set-leader-keys "s w i" 'engine/search-spacemacs-issues)
(spacemacs/set-leader-keys "s w M" 'engine/search-google-maps)
(spacemacs/set-leader-keys "s w b" 'engine/search-project-gutenberg)
(spacemacs/set-leader-keys "s w a" 'engine/search-wolfram-alpha)
(spacemacs/set-leader-keys "s w w" 'engine/search-wikipedia)
(spacemacs/set-leader-keys "s w m" 'engine/search-melpa)

(spacemacs/set-leader-keys "swh" 'browser-hist-search)

;; (global-set-key (kbd "C-h C-l") 'command-log-toggle)
;; (global-set-key (kbd "C-h C-k") 'keycast-tab-bar-mode)

(global-set-key (kbd "C-h C-l") (lambda () (interactive) (display-buffer ilog-buffer-name)))
(spacemacs/set-leader-keys "TL" #'(lambda () (interactive) (display-buffer ilog-buffer-name)))
```


## <span class="section-num">4</span> <kbd>After</kbd> User-Configurations {#h:b714e337-bcfc-4ba4-884e-c7de9486d1ce}




### <span class="section-num">4.1</span> Configurations (`user-configs.el`) {#h:96d2754f-45b2-417b-a7f0-58749a389c08}


#### <span class="section-num">4.1.1</span> Custom 'defaults' {#h:b245ad55-c4c9-4e17-9ae6-607be98a1bec}

```elisp
;;; -*- mode: emacs-lisp; lexical-binding: t -*-

;;;; Custom 'defaults'

;;;;; default configs

;; Line should be 80 characters wide, not 72
(setq-default
 ;; ====== Buffer-local variables ======
 ;; Display long lines
 truncate-lines nil
 ;; Default fill column width
 fill-column 80 ;; default 70
 ;; Never mix, use only spaces
 indent-tabs-mode nil

 ;; Width for line numbers
 ;; display-line-numbers-width 4
 display-line-numbers-width-start t
 ;; Make TABs be displayed with a width of 2.
 tab-width 4 ;; 2023-12-25 prot
 )

;; Tab first tries to indent the current line, and if the line was already
;; indented, then try to complete the thing at point.
;; (setq tab-always-indent 'complete)

;; Force packages relying on this general indentation variable (e.g., lsp-mode) to indent with 2 spaces.
;; (setq-default standard-indent 2)

;; Ridiculous path view is vanilla emacs. change truename!
;; truename 을 원하지 않는다. 심볼링링크대로 쓰고 싶다면 nil
(setq find-file-visit-truename t)

;; (setq auto-save-list-file-prefix "~/spacemacs/.cache/auto-save/")
(setq auto-save-interval 1500
      auto-save-timeout 180
      auto-save-visited-interval 90)

(setq recentf-max-saved-items 200) ; default 20
(unless recentf-mode (recentf-mode 1))

(setq
 ;; sh-shell 'bash
 ;; sh-indentation
 sh-basic-offset tab-width)

(setq
 ;; ====== Default behavior ======
 ;; Inhibit startup message
 inhibit-splash-screen t
 inhibit-startup-message t ; default nil
 ;; Do not ring
 ;; ring-bell-function 'ignore
 ;; Increase the large file threshold to 50 MiB
 large-file-warning-threshold (* 50 1024 1024)

 ;; Initial scratch message (will be overridden if "fortune" is installed)
 ;; initial-scratch-message ";; MinEmacs -- start here!"
 ;; Set initial buffer to fundamental-mode for faster load
 ;; initial-major-mode 'fundamental-mode

 ;; Always prompt in minibuffer (no GUI)
 use-dialog-box nil
 ;; Use y or n instead of yes or no
 use-short-answers t
 ;; Confirm before quitting
 confirm-kill-emacs 'y-or-n-p
 ;; Filter duplicate entries in kill ring
 kill-do-not-save-duplicates t
 ;; Save existing clipboard text into the kill ring before replacing it.
 save-interprogram-paste-before-kill t

 ;; Save files only in sub-directories of current project
 ;; save-some-buffers-default-predicate 'save-some-buffers-root

 ;; Use single space between sentences
 sentence-end-double-space nil
 ;; Move stuff to trash
 delete-by-moving-to-trash t
 ;; trash-directory "~/.Trash"

 ;; Select help window for faster quit!
 help-window-select t

 ;; FIXME More info on completions
 completions-detailed t

 ;; Do not ask obvious questions, follow symlinks
 vc-follow-symlinks t

 ;; Kill the shell buffer after exit
 shell-kill-buffer-on-exit t

 ;; ====== Passwords and encryption ======
 ;; Default auth-sources to GPG
 auth-sources '("~/.authinfo.gpg")
 ;; Enable password caching
 password-cache t
 ;; 10 minutes, default is 16 sec
 password-cache-expiry 600
 ;; Enable caching, do not keep asking about GPG key
 auth-source-do-cache t
 ;; All day, default is 2h (7200)
 auth-source-cache-expiry 86400

 ;; ====== Performances ======
 ;; Don’t compact font caches during GC
 inhibit-compacting-font-caches t
 ;; Increase single chunk bytes to read from subprocess (default 4096)
 read-process-output-max (if (eq system-type 'gnu/linux)
                             (condition-case nil
                                 ;; Android may raise permission-denied error
                                 (with-temp-buffer
                                   (insert-file-contents
                                    "/proc/sys/fs/pipe-max-size")
                                   (string-to-number (buffer-string)))
                               ;; If an error occured, fallback to the default value
                               (error read-process-output-max))
                           (* 1024 1024))

 ;; TODO 2023-06-19 왜 갑자기 클라이언트 프레임 사이즈가 이상하지?!
 ;; Do force frame size to be a multiple of char size
 frame-resize-pixelwise t

 ;; ;; Emacsclient does not use full frame size (NIL 필수!)
 frame-inhibit-implied-resize nil

 ;; Stretch cursor to the glyph width
 ;; make cursor the width of the character it is under
 ;; i.e. full width of a TAB
 x-stretch-cursor t
 ;; Show trailing whitespaces
 show-trailing-whitespace t
 ;; Resize window combinations proportionally
 window-combination-resize t
 ;; Enable time in the mode-line
 ;; display-time-string-forms '((propertize (concat 24-hours ":" minutes)))
 ;; No ugly button for widgets
 widget-image-enable nil
 ;; Show unprettified symbol under cursor (when in `prettify-symbols-mode')
 ;; prettify-symbols-unprettify-at-point t
 ;; Make tooltips last a bit longer (default 10s)
 tooltip-hide-delay 20
 ;; Use small frames to display tooltips instead of the default OS tooltips
 use-system-tooltips nil

 ;; ====== Undo ======
 ;; 10MB (default is 160kB)
 undo-limit 10000000
 ;; 50MB (default is 240kB)
 undo-strong-limit 50000000
 ;; 150MB (default is 24MB)
 undo-outer-limit 150000000

 ;; ====== Editing ======
 ;; Default behavior for `whitespace-cleanup'
 ;; whitespace-action '(cleanup auto-cleanup)
 ;; End files with newline
 require-final-newline t

 ;; Enable Drag-and-Drop of regions
 mouse-drag-copy-region t
 mouse-drag-and-drop-region t
 ;; Enable Drag-and-Drop of regions from Emacs to external programs
 mouse-drag-and-drop-region-cross-program t

 ;; ====== Scrolling ======
 ;; Do not adjust window-vscroll to view tall lines
 auto-window-vscroll nil
 ;; Keep the point in the same position while scrolling
 scroll-preserve-screen-position t
 ;; Do not move cursor to the center when scrolling
 scroll-conservatively 101
 ;; Scroll at a margin of one line
 scroll-margin 1
 ;; Better scrolling on Emacs29+, specially on a touchpad
 pixel-scroll-precision-use-momentum t

 column-number-mode t ; default nil

 ;; 복붙만 한다.
 ;; ;; ====== Compilation ======
 ;; ;; Scroll compilation buffer
 ;; compilation-scroll-output t ; 'first-error can be a good option
 ;; ;; Always kill current compilation process before starting a new one
 ;; compilation-always-kill t
 ;; ;; Skip visited messages on compilation motion commands
 ;; compilation-skip-visited t
 ;; ;; Keep it readable
 ;; compilation-window-height 12
 ) ; end-of-setq

;; Kill minibuffer when switching by mouse to another window
;; Taken from: https://trey-jackson.blogspot.com/2010/04/emacs-tip-36-abort-minibuffer-when.html
;; (add-hook
;;  'mouse-leave-buffer-hook
;;  (defun +minibuffer--kill-on-mouse-h ()
;;    "Kill the minibuffer when switching to window with mouse."
;;    (when (and (>= (recursion-depth) 1) (active-minibuffer-window))
;;      (abort-recursive-edit))))

;; Scroll pixel by pixel, in Emacs29+ there is a more pricise mode way to scroll
(if (>= emacs-major-version 29)
    (pixel-scroll-precision-mode 1)
  (pixel-scroll-mode 1))

;; Files with known long lines
;; SPC f l to open files literally to disable most text processing
;; So long mode when Emacs thinks a file would affect performance
(global-so-long-mode 1)

;; Easily navigate sillycased words
(global-subword-mode 1)

;; Emacs text rendering optimizations
;; https://200ok.ch/posts/2020-09-29_comprehensive_guide_on_handling_long_lines_in_emacs.html
;; Only render text left to right
(setq-default bidi-paragraph-direction 'left-to-right)

;; 라인 컬럼 보여주는 검은 세로선
;; (when (display-graphic-p) ; gui
;;   (global-display-fill-column-indicator-mode))

;; /spacemacs/core/libs/ido-vertical-mode.el
;; 찾아서 꺼줘야 한다. Spacemacs 에서 자동으로 켜놓았네.
(ido-vertical-mode -1)

;; http://yummymelon.com/devnull/surprise-and-emacs-defaults.html
;;텍스트를 선택한 다음 그 위에 입력하면 해당 텍스트가 삭제되어야 합니다.
;;놀랍게도 기본 Emac 에서는 이 동작이 기본적으로 제공되지 않습니다. 명시적으로
;;활성화해야 합니다.
(setq delete-selection-mode t) ; default nil
;; (setq magit-save-repository-buffers 'dontask) ; default t

;; Show a message when garbage collection happens? Useful while tuning the GC
;; (setq garbage-collection-messages t)
;; (add-function :after
;;               after-focus-change-function
;;               (lambda () (unless (frame-focus-state)
;;                            (garbage-collect))))

;; ====== Recent files ======
;; Increase the maximum number of saved items
;; Ignore case when searching recentf files
(setq recentf-case-fold-search t)
;; Exclude some files from being remembered by recentf
(setq recentf-max-saved-items 200) ; default 20

(setq recentf-exclude nil)
;; (add-to-list 'recentf-exclude (recentf-expand-file-name package-user-dir))
(add-to-list 'recentf-exclude
             (recentf-expand-file-name spacemacs-cache-directory))
(add-to-list 'recentf-exclude "COMMIT_EDITMSG\\'")
(when custom-file
  (add-to-list 'recentf-exclude (recentf-expand-file-name custom-file)))
(add-to-list 'recentf-exclude ".gz")
(add-to-list 'recentf-exclude ".elc")

;; Show recursion depth in minibuffer (see `enable-recursive-minibuffers')
(minibuffer-depth-indicate-mode 1)

;; default 120 emacs-29, 60 emacs-28
(setq kill-ring-max 30) ; keep it small

;; automatically revert buffers for changed files
(setq auto-revert-interval 5) ; default 5

;; 시간 표시 형식은 영어로 표시해서 호환성을 높입니다.
(setq system-time-locale "C")

;; Disable .# lock files
(setq create-lockfiles nil)

;; Shr group: Simple HTML Renderer 를 의미한다. 여기 설정을 바꾸면 faces 를 수정할 수 있음
(setq shr-use-fonts nil)

;; buffer size 를 표기 합니다.
(setq size-indication-mode t)

;; turn off page-break-lines-mode for org-roam
(global-page-break-lines-mode -1)
;; (global-visual-line-mode +1)

;; (tooltip-mode -1)           ; Disable tooltips
(tool-bar-mode -1)          ; Disable the toolbar

(unless *is-termux*
  (scroll-bar-mode -1))

;; 2023-04-16 Learn how-to use menu-bar for beginner on GUI mode
(when (display-graphic-p) ; gui
  (menu-bar-mode -1)          ; Disable the menu bar
  ;; Read 'The Forgotten History of the Blinking Cursor'
  (blink-cursor-mode 1)
  (set-fringe-mode 10) ;; Give some breathing room
  (tooltip-mode 1)

  ;; For my mouse that also has left - right mouse scroll
  (setq mouse-wheel-tilt-scroll t)
  (setq mouse-wheel-scroll-amount-horizontal 2)
  (setq mouse-wheel-flip-direction nil) ; default nil

  ;; 2023-12-18 fill-column-indicator off
  ;; (add-hook 'org-mode-hook 'display-fill-column-indicator-mode)
  ;; (add-hook 'markdown-mode-hook 'display-fill-column-indicator-mode)
  )

(unless (display-graphic-p) ; terminal
  (menu-bar-mode -1)          ; Disable the menu bar
  (blink-cursor-mode -1)
  (gpm-mouse-mode -1)
  (xterm-mouse-mode -1)
  (mouse-wheel-mode -1)
  (pixel-scroll-precision-mode -1)

  (setq all-the-icons-color-icons nil)

  (setq mouse-wheel-follow-mouse -1)
  (setq
   mouse-drag-and-drop-region nil
   mouse-drag-and-drop-region-cross-program nil
   auto-window-vscroll nil
   fast-but-imprecise-scrolling nil
   scroll-preserve-screen-position nil
   pixel-scroll-precision-use-momentum nil
   )

  (setq evil-motions nil)
  )

;;;;; vertico hangul

;; 2023-11-07 버벅이는 문제로 끈다.
;; (with-eval-after-load 'vertico
;;     (defun my/vertico-setup-then-remove-post-command-hook (&rest args)
;;       "vertico--setup 함수에서 추가하는 post-command-hook 을 제거한다.
;; 입력 조합으로 표현하는 한글 입력시 post-command-hook 이 입력되지
;; 않는다. 한글 증분 완성을 위해 timer 로 호출하기 때문에 제거한다"
;;       (remove-hook 'post-command-hook #'vertico--exhibit 'local))

;;     (defun my/vertico-exhibit-with-timer (&rest args)
;;       "타이머를 넣어 타이머 이벤트 발생시 vertico--exhibit 을 호출해
;;  미니버퍼 완성(completion) 후보 리스트를 갱신한다
;; post-command-hook 이 발동하지 않는 한글 입력시에도 한글 증분
;; 완성을 하기 위해 timer 를 사용한다"
;;       (let (timer)
;;         (unwind-protect
;;             (progn
;;               (setq timer (run-with-idle-timer
;;                            0.01
;;                            'repeat
;;                            (lambda ()
;;                              (with-selected-window (or (active-minibuffer-window)
;;                                                        (minibuffer-window))
;;                                (vertico--exhibit))
;;                              )))
;;               (apply args))
;;           (when timer (cancel-timer timer)))))

;;     (advice-add #'vertico--setup :after #'my/vertico-setup-then-remove-post-command-hook)
;;     (advice-add #'vertico--advice :around #'my/vertico-exhibit-with-timer)
;;     )

;;;;; CJK Word Wrap

;; Emacs 28 adds better word wrap / line break support for CJK.
(setq word-wrap-by-category t) ; default nil

;; Normally when word-wrap is on, Emacs only breaks lines after
;; whitespace characters.  When this option is turned on, Emacs also
;; breaks lines after characters that have the "|" category (defined in
;; characters.el).  This is useful for allowing breaking after CJK
;; characters and improves the word-wrapping for CJK text mixed with
;; Latin text.

;; 일반적으로 단어 줄 바꿈이 켜져 있으면 Emac 은 공백 문자 뒤에 오는 줄만 줄
;; 바꿈합니다. 이 옵션을 켜면 Emac 은 "|" 범주(characters.el 에 정의됨)가 있는 문자
;; 뒤의 줄도 줄 바꿈합니다. 이 옵션은 한중일 문자 뒤에 줄 바꿈을 허용하는 데
;; 유용하며 라틴 텍스트와 혼합된 한중일 텍스트의 단어 줄 바꿈을 개선합니다.

;;;;; hungry-delete-backward and forward

;; layers/+emacs/better-defaults/keybindings.el
(defun spacemacs/backward-kill-word-or-region (&optional arg)
  "Calls `kill-region' when a region is active and
`backward-kill-word' otherwise. ARG is passed to
`backward-kill-word' if no region is active."
  (interactive "p")
  (if (region-active-p)
      ;; call interactively so kill-region handles rectangular selection
      ;; correctly (see https://github.com/syl20bnr/spacemacs/issues/3278)
      (call-interactively #'kill-region)
    (backward-kill-word arg)))

;; 익숙한 키 바인딩이라. 그냥 두자.
(global-set-key (kbd "M-<backspace>") 'spacemacs/backward-kill-word-or-region)

;; [x] 모드와 상관 없이 backsapce 는 delete-backward-char 안된다. : conflict
;; 모드와 상관 없이 Delete 키는 delete-forward-char : default

(with-eval-after-load 'hungry-delete
  (define-key hungry-delete-mode-map (kbd "S-<backspace>") 'hungry-delete-backward)
  (define-key hungry-delete-mode-map (kbd "S-<delete>") 'hungry-delete-forward)
  (define-key hungry-delete-mode-map (kbd "S-DEL") 'hungry-delete-forward)
  )

;; 기본 스타일 바인딩을 사용하자.
(global-set-key (kbd "S-<backspace>") 'hungry-delete-backward) ; default bindings
(global-set-key (kbd "S-<delete>") 'hungry-delete-forward)
(global-set-key (kbd "S-DEL") 'hungry-delete-forward)

;; C 로 하려다가 기본이 S 더라. 기본으로 가자.
;; (global-set-key (kbd "C-<backspace>") 'hungry-delete-backward)
;; (global-set-key (kbd "C-<delete>") 'hungry-delete-forward)
;; (global-set-key (kbd "C-DEL") 'hungry-delete-forward)

(global-hungry-delete-mode t)

;;;;; winner-mode

(require 'winner)
(setq winner-boring-buffers-regexp "\\*.*\\*")
(winner-mode +1)
;; C-c <left>   winner-undo
;; C-c <right>  winner-redo
(define-key evil-window-map "u" 'winner-undo)
(define-key evil-window-map "U" 'winner-redo)
;; (define-key winner-mode-map (kbd "M-s-[") #'winner-undo)
;; (define-key winner-mode-map (kbd "M-s-]") #'winner-redo)

;;;;; trailing-whitespace and check large file

;; 나는 문서는 정리하는게 좋다.
(defun kimim/save-buffer-advice (orig-fun &rest arg)
  ;; (when (not (memq major-mode '(org-mode markdown-mode text-mode)))
  (when (memq major-mode '(org-mode))
    (delete-trailing-whitespace))
  (apply orig-fun arg))
(advice-add 'save-buffer :around #'kimim/save-buffer-advice)
;; (setq save-silently t) ;; default nil

;; (use-package simple
;;   :config
;;   (setq column-number-mode t
;;         ;; comment-empty-lines nil ; default nil
;;         ;; delete-trailing-lines nil ; default t
;;         ;; eval-expression-print-length nil
;;         ;; eval-expression-print-level nil
;;         next-error-message-highlight t
;;         ;; save clipboard contents into kill-ring before replace them
;;         save-interprogram-paste-before-kill t))

;; (use-package files
;;   :hook ((before-save . delete-trailing-whitespace)
;;          (after-save . executable-make-buffer-file-executable-if-script-p))
;;   :init
;;   (setq make-backup-files nil        ;; don't create backup~ files
;;         revert-without-query '(".*") ;; disable revert query
;;         enable-remote-dir-locals t)
;;   :config
;;   ;; Prompt to open file literally if large file.
;;   ;; (defun check-large-file ()
;;   ;;   "Check when opening large files - literal file open."
;;   ;;   (let* ((filename (buffer-file-name))
;;   ;;          (size (nth 7 (file-attributes filename))))
;;   ;;     (when (and
;;   ;;            (not (memq major-mode
;;   ;;                       '(archive-mode doc-view-mode doc-view-mode-maybe
;;   ;;                                      ebrowse-tree-mode emacs-lisp-mode
;;   ;;                                      fundamental-mode git-commit-mode
;;   ;;                                      image-mode jka-compr pdf-view-mode
;;   ;;                                      ;; org-mode
;;   ;;                                      tags-table-mode tar-mode)))
;;   ;;            size (> size (* 1024 1024 5))
;;   ;;            (y-or-n-p (format (concat "%s is a large file, open literally to "
;;   ;;                                      "avoid performance issues?")
;;   ;;                              filename)))
;;   ;;       (setq buffer-read-only t)
;;   ;;       (buffer-disable-undo)
;;   ;;       (fundamental-mode))))
;;   ;; (add-hook 'find-file-hook #'check-large-file)

;;   ;; see document of `move-file-to-trash'
;;   ;; sudo apt-get install trash-cli
;;   (defun system-move-file-to-trash (filename)
;;     (process-file-shell-command
;;      (format "trash %S" (file-local-name filename))))

;;   (defun make-directory-maybe ()
;;     "Create parent directory if not exists while visiting file."
;;     (let ((dir (file-name-directory buffer-file-name)))
;;       (unless (file-exists-p dir)
;;         (if (y-or-n-p (format "Directory %s does not exist,do you want you create it? " dir))
;;             (make-directory dir t)
;;           (keyboard-quit)))))
;;   (add-to-list 'find-file-not-found-functions 'make-directory-maybe nil #'eq))

(defun my/describe-thing-in-popup ()
  (interactive)
  (let* ((thing (symbol-at-point))
         (help-xref-following t)
         (description (with-temp-buffer
                        (help-mode)
                        (help-xref-interned thing)
                        (buffer-string))))
    (popup-tip description
               :point (point)
               :around t
               :height 30
               :scroll-bar t
               :margin t)))
(spacemacs/set-leader-keys "hh" 'my/describe-thing-in-popup)

;; ((file location) (file location))
;;   1              2
;; (defvar kimim/last-edit-list nil)

;; (defun kimim/backward-last-edit ()
;;   (interactive)
;;   (let ((position (car kimim/last-edit-list)))
;;     (when position
;;       (print position)
;;       ;;(print kimim/last-edit-list)
;;       (find-file (car position))
;;       (goto-char (cdr position))
;;       (setq kimim/last-edit-list (cdr kimim/last-edit-list)))))

;; ;; TODO shrink list if more items
;; (defun kimim/buffer-edit-hook (beg end len)
;;   (interactive)
;;   (let ((bfn (buffer-file-name)))
;;     ;; insert modification in current index
;;     ;; remove forward locations
;;     ;; if longer than 100, remove old locations
;;     (when bfn
;;       (progn
;;         (add-to-list 'kimim/last-edit-list (cons bfn end))))))

;; (add-hook 'after-change-functions 'kimim/buffer-edit-hook)

;; (global-set-key (kbd "M-`") 'kimim/backward-last-edit)
;; (spacemacs/set-leader-keys "jg" 'kimim/backward-last-edit)

;;;;; time-stamp

(require 'time-stamp)
(add-hook 'write-file-functions 'time-stamp) ; update when saving

;;;;; User Goto Functions

(defun goto-emacs-dotfiles.org ()
  "Open jh-emacs.org file."
  (interactive)
  (find-file (concat dotspacemacs-directory "jh-emacs.org")))

(defun goto-pandoc-config ()
  "Open pandoc metadata file."
  (interactive)
  (find-file "~/.config/pandoc/metadata.yml"))

;;;;; show-paren-mode/electric-pair-mode and customize for org-mode

;; Turn off electric-indent-mode
;; 2023-11-10 puni + electric-pair 사용 중. 이걸 꺼야 org-block 에서 문제가 없다.
;; 2023-09-28 아니다. 켜 놓은 이유가 있을 것. elctric-pair 가 아니지 않는가?
;; 스페이스맥스에서 왜 이걸 켜 놓는 것인가?! 일단 끈다.
;; C-j 누르면 electric-newline-and-maybe-indent 수행. indent 가 안맞는다. 필요 없다.
(electric-indent-mode -1) ; important!! 이렇게 따로 꺼야 한다.

;; Other Options
;; https://github.com/alphapapa/smart-tab-over

;; https://www.gnu.org/software/emacs/manual/html_node/emacs/Matching.html
;; 괄호만 강조
(setq show-paren-style 'parenthesis) ; default 'parenthesis
;; 괄호 입력 후 내용 입력시 괄호를 강조
(setq show-paren-when-point-inside-paren t)
;; (setq show-paren-when-point-in-periphery t)

;; 괄호 강조를 즉시 보여준다
(setq show-paren-delay 0) ; 0.125
(show-paren-mode t)

;; https://www.gnu.org/software/emacs/manual/html_node/emacs/Matching.html
;; 괄호, 구분자(delimiter) 자동 쌍 맞추기
(setq electric-pair-pairs '((?\{ . ?\})
                            (?\( . ?\))
                            (?\[ . ?\])
                            (?\" . ?\")))

;; Disable auto-pairing of "<" or "[" in org-mode with electric-pair-mode
(defun my/org-enhance-electric-pair-inhibit-predicate ()
  "Disable auto-pairing of \"<\" or \"[\" in `org-mode' when using `electric-pair-mode'."
  (when (and electric-pair-mode (eql major-mode #'org-mode))
    (setq-local electric-pair-inhibit-predicate
                `(lambda (c)
                   (if (or (char-equal c ?<)
                           (char-equal c ?\[ ))
                       t (,electric-pair-inhibit-predicate c))))))

;; Add hook to both electric-pair-mode-hook and org-mode-hook
;; This ensures org-mode buffers don't behave weirdly,
;; no matter when electric-pair-mode is activated.
(add-hook 'electric-pair-mode-hook #'my/org-enhance-electric-pair-inhibit-predicate)
(add-hook 'org-mode-hook #'my/org-enhance-electric-pair-inhibit-predicate)

;;;;; Corfu and electric-Pair and Jump In/Out Parens

;; Linux GUI : <tab> TAB
;; Linux Terminal : TAB
;; Linux GUI : S-<iso-lefttab>
;; Linux Terminal : <backtab>

  ;;;###autoload
(defun jump-out-of-pair ()
  (interactive)
  (let ((found (search-forward-regexp "[])}\"'`*=]" nil t)))
  	(when found
  	  (cond ((or (looking-back "\\*\\*" 2)
  		         (looking-back "``" 2)
  		         (looking-back "\"\"" 2) ; 2023-10-02 added
  		         (looking-back "''" 2)
  		         (looking-back "==" 2))
  			 (forward-char))
  			(t (forward-char 0))))))
;; 절대 하지 말것! (global-set-key [remap indent-for-tab-command] #'jump-out-of-pair)

  ;;;###autoload
(defun jump-backward-pair ()
  (interactive)
  (let ((found (search-backward-regexp "[])}\"'`*=]" nil t)))
    (when found
      (cond ((or (looking-back "\\*\\*" 2)
                 (looking-back "``" 2)
                 (looking-back "\"\"" 2) ; 2023-10-02 added
                 (looking-back "''" 2)
                 (looking-back "==" 2))
             (backward-char))
            (t (backward-char 0))))))

;; Keybindings
;; 자동 완성 하지 않고 다음 줄 - C-<return>
;; 자동 완성 하지 않고 괄호 점프 - Tab
;; 자동 완성 하지 않고 현재 위치 - C-q : corfu-quit
;; 자동 완성 하지 않고 다음 위치 - Space
;; 자동 완성 - <return>

;;   ;; Tab 이 자동 완성이면 괄호 점프랑 충돌 난다.
;;   ;; C-j/k C-n/p 는 직관적인 기본 설정이므로 건들이지 않는다.
(with-eval-after-load 'corfu
  (evil-define-key '(insert) org-mode-map (kbd "C-M-<return>") 'jump-out-of-pair)
  (evil-define-key '(insert) prog-mode-map (kbd "C-M-<return>") 'jump-out-of-pair)

  (evil-define-key '(insert) prog-mode-map (kbd "<tab>") 'jump-out-of-pair)
  (evil-define-key '(insert) prog-mode-map (kbd "TAB") 'jump-out-of-pair)
  (evil-define-key '(insert) corfu-map (kbd "<tab>") 'jump-out-of-pair)
  (evil-define-key '(insert) corfu-map (kbd "TAB") 'jump-out-of-pair)

  ;; (define-key prog-mode-map (kbd "<backtab>") 'jump-backward-pair)
  (evil-define-key '(insert) prog-mode-map (kbd "<backtab>") 'jump-backward-pair)
  (evil-define-key '(insert) prog-mode-map (kbd "S-<iso-lefttab>") 'jump-backward-pair)
  (evil-define-key '(insert) corfu-map (kbd "<backtab>") 'jump-backward-pair)
  (evil-define-key '(insert) corfu-map (kbd "S-<iso-lefttab>") 'jump-backward-pair)

  (evil-define-key '(insert) corfu-map (kbd "C-<return>") 'newline-and-indent) ;; <C-return>
  (evil-define-key '(insert) prog-mode-map (kbd "C-<return>") 'newline-and-indent) ;; <C-return>

  ;;     ;; M-g                             corfu-info-location
  ;;     ;; M-h                             corfu-info-documentation
  )

;;  ;; (define-key corfu-map (kbd "S-TAB") 'jump-backward-pair)
;;  ;; (define-key prog-mode-map (kbd "S-TAB") 'jump-backward-pair)
;;  ;; (define-key prog-mode-map (kbd "S-<tab>") 'jump-backward-pair)
;;  ;; (define-key prog-mode-map (kbd "S-TAB") 'jump-backward-pair)

;;;;; eldoc

(setq eldoc-echo-area-use-multiline-p nil) ; important

;; eldoc-echo-area-prefer-doc-buffer t ; default nil - aloway show echo-area
;; ;; eldoc-display-functions '(eldoc-display-in-echo-area eldoc-display-in-buffer)
;; ;; eldoc-documentation-strategy 'eldoc-documentation-compose)

(defun eldoc-toggle ()
  "Toggle eldoc's documentation buffer."
  (interactive)
  (let ((buffer (eldoc-doc-buffer)))
    (if-let (w (and buffer (get-buffer-window buffer)))
        (delete-window w)
      (eldoc-doc-buffer t))))
(global-set-key (kbd "C-M-'") 'eldoc-toggle)

;;;;; yasnippet Navigation M-n/M-p

;; use Meta-n and Meta-p to jump between fields
(with-eval-after-load 'yasnippet
  (define-key yas-keymap (kbd "M-n") 'yas-next-field-or-maybe-expand)
  (define-key yas-keymap (kbd "M-p") 'yas-prev-field))

;;;;; evil-org

;; https://emacs.stackexchange.com/questions/39434/evil-dont-yank-with-only-whitespace-to-register/53536#53536
(with-eval-after-load 'evil-org
  (define-key evil-normal-state-map "x" 'delete-forward-char)
  (define-key evil-normal-state-map "X" 'delete-backward-char)
  (evil-define-key 'normal 'evil-org-mode "x" 'delete-forward-char)
  (evil-define-key 'normal 'evil-org-mode "X" 'delete-backward-char)
  )

;;;;; evil + hangul

;; 노멀로 빠지면 무조건 영어로 변경
(defun my/turn-off-input-method (&rest _)
  (if current-input-method
      (when (derived-mode-p 'prog-mode) ;; only prog-mode
        (deactivate-input-method))))

(advice-add 'evil-normal-state :before #'my/turn-off-input-method)
(mapc (lambda (mode)
        (let ((keymap (intern (format "evil-%s-state-map" mode))))
          (define-key (symbol-value keymap) [?\S- ]
                      #'(lambda () (interactive)
                          (message
                           (format "Input method is disabled in %s state." evil-state))))))
      '(motion normal visual))

;;;;; TODO Multiple-Cursors

;; (use-package
;;   multiple-cursors
;;   :bind ("C->" . 'mc/mark-next-like-this) ("C-<" . 'mc/mark-previous-like-this))

;;;;; DONT consult-gh and consult-dash

;; (with-eval-after-load 'consult-gh
;;   (add-to-list 'savehist-additional-variables 'consult-gh--known-orgs-list) ;;keep record of searched orgs
;;   (add-to-list 'savehist-additional-variables 'consult-gh--known-repos-list)) ;;keep record of searched repos

(with-eval-after-load 'consult-dash
  (define-key consult-dash-embark-keymap (kbd "b") #'browse-url)
  ;; Use the symbol at point as initial search term
  (consult-customize consult-dash :initial (thing-at-point 'symbol))
  )

;;;;; bookmark

(with-eval-after-load 'bookmark
  (setq bookmark-default-file (concat org-directory "var/bookmarks"))
  (setq bookmark-use-annotations nil)
  (setq bookmark-automatically-show-annotations t)
  ;; (setq bookmark-fringe-mark t) ; 29.1 default 'bookmark-mark
  (add-hook 'bookmark-bmenu-mode-hook #'hl-line-mode)
  )


;;;;; savehist

(add-to-list 'savehist-additional-variables 'citar-history)

;;;;; pulse

;; add visual pulse when changing focus, like beacon but built-in
;; from from https://karthinks.com/software/batteries-included-with-emacs/
(defun pulse-line (&rest _)
  "Pulse the current line."
  (pulse-momentary-highlight-one-line (point)))
(dolist (command '(scroll-up-command
                   scroll-down-command
                   recenter-top-bottom
                   other-window))
  (advice-add command :after #'pulse-line))

;;;;; visual-line-mode with popwin

;; /home/junghan/sync/man/dotsamples/vanilla/localauthor-dotfiles-zk/init.el:119
;; (with-current-buffer "*Messages*"
;;   (visual-line-mode))
;; (with-current-buffer "*scratch*"
;;   (visual-line-mode))

(add-hook 'compilation-mode-hook 'visual-line-mode)
;; (add-hook 'fundamental-mode-hook 'visual-line-mode)

;;;;; Performance

;;;;;;  Make cursor movement an order of magnitude faster
(setq fast-but-imprecise-scrolling t)
;; NOTE: setting this to `0' like it was recommended in the article above seems
;; to cause fontification to happen in real time, which can be pretty slow in
;; large buffers. Giving it a delay seems to be better.
;; (setq jit-lock-defer-time 0.05) ; 0.25

```


#### <span class="section-num">4.1.2</span> 'Writing' - org-mode {#h:fd6b2425-1634-4fe1-bff2-764bef40964d}

```elisp
;;;; 'Writing' - org-mode

;;;;; org-gcal
(require 'org-gcal)
;; (dolist (caltemplate
;;           `(
;;              ;; org-gcal
;;              ("G" "(G) Google Calendar")
;;              ("Go" "work (gcal-office)" entry (file "~/sync/org/roam/workflow/gcal-office.org")
;;                "* %?\n:PROPERTIES:\n:calendar-id:\tjunghanacs@gmail.com\n:END:\n:org-gcal:\n%^T--%^T\n:END:\n\n" :jump-to-captured t)
;;              ("Gh" "private (gcal-home)" entry (file "~/sync/org/roam/workflow/gcal-home.org")
;;                "* %?\n:PROPERTIES:\n:calendar-id:\te07727dc2c9e2a565eb162c45cfd31796acefc04de10540cb84a439de2fabe54@group.calendar.google.com\n:END:\n:org-gcal:\n%^T--%^T\n:END:\n\n"
;;                :jump-to-captured t)))
;;   (add-to-list 'org-capture-templates caltemplate))

;;;;; DONT org and markdown with variable-pitch

;; [2023-12-19 Tue 12:34] 나중에 해보자.
;; (when (display-graphic-p) ; gui

;;   (defun my/enable-variable-pitch-markdown-mode ()
;;     (interactive)
;;     (variable-pitch-mode 1) ;; 모든 글꼴 변환
;;     (mapc
;;      (lambda (face)
;;        (set-face-attribute face nil :inherit 'fixed-pitch))
;;      (list 'markdown-code-face
;;            'markdown-header-face
;;            'markdown-table-face
;;            'markdown-markup-face
;;            'markdown-math-face
;;            'markdown-language-keyword-face
;;            )))
;;   ;; (add-hook 'markdown-mode-hook #'my/enable-variable-pitch-markdown-mode)
;;   )

;;;;; repeated words

(defun the-the ()
  "Search forward for for a duplicated word."
  (interactive)
  (message "Searching for for duplicated words ...")
  (push-mark)
  ;; This regexp is not perfect
  ;; but is fairly good over all:
  (if (re-search-forward
       "\\b\\([^@ \n\t]+\\)[ \n\t]+\\1\\b" nil 'move)
      (message "Found duplicated word.")
    (message "End of buffer")))

;;   ;; Bind 'the-the' to  C-c \
;;   (bind-key "C-c \\" 'the-the)

;;;;; add-newlines-between-paragraphs

(defun add-newlines-between-paragraphs ()
  (interactive)
  (save-excursion
    (beginning-of-buffer)
    (while (< (point) (point-max))
      (move-end-of-line nil)
      (newline)
      (next-line))))

;;;;; org-cliplink

(setq org-cliplink-max-length 72)
(setq org-cliplink-ellipsis "-")

;; from ohyecloudy
(defun my/org-cliplink ()
  (interactive)
  (org-cliplink-insert-transformed-title
   (org-cliplink-clipboard-content)     ;take the URL from the CLIPBOARD
   #'my-org-link-transformer))

(defun my-org-link-transformer (url title)
  (let* ((parsed-url (url-generic-parse-url url)) ;parse the url
         (host-url (replace-regexp-in-string "^www\\." "" (url-host parsed-url)))
         (clean-title
          (cond
           ;; if the host is github.com, cleanup the title
           ((string= (url-host parsed-url) "github.com")
            (replace-regexp-in-string "^/" ""
                                      (car (url-path-and-query parsed-url))))
           ;; otherwise keep the original title
           (t (my-org-cliplink--cleansing-site-title title))))
         (title-with-url (format "%s - %s" clean-title host-url)))
    ;; forward the title to the default org-cliplink transformer
    (org-cliplink-org-mode-link-transformer url title-with-url)))

(defun my-org-cliplink--cleansing-site-title (title)
  (let ((result title)
        (target-site-titles '(" - 위키백과, 우리 모두의 백과사전"
                              " - Wikipedia"
                              " - PUBLY"
                              " - YES24"
                              "알라딘: "
                              " : 클리앙"
                              " - YouTube")))
    (dolist (elem target-site-titles)
      (if (string-match elem result)
          (setq result (string-replace elem "" result))
        result))
    result))

;; 마지막에 host 를 붙이고 싶어서 link transformer 함수를 짰다. =title -
;; ohyecloudy.com= 식으로 org link 를 만든다.
(define-key org-mode-map [remap org-cliplink] 'my/org-cliplink)

;;;;; org-babel-load-languages

(add-to-list 'org-babel-load-languages '(shell . t))
(add-to-list 'org-babel-load-languages '(awk . t))
(add-to-list 'org-babel-load-languages '(mermaid . t))
(add-to-list 'org-babel-load-languages '(racket . t))
(add-to-list 'org-babel-load-languages '(sed . t))
(add-to-list 'org-babel-load-languages '(js . t))

;; (add-to-list 'org-babel-load-languages '(elixir . t))
;; (add-to-list 'org-babel-load-languages '(d2 . t))

;;;;; inline Latex Fragment preview nil

;; math-preview 이용

;; https://whhone.com/emacs-config/#hydra-consult-menu
;; To toggle the preview, use M-x org-latex-preview or C-c C-x C-l.
;; To enable preview on startup for an org file, add #+STARTUP: latexpreview to the header.
;; To enable preview on startup globally, add (setq org-startup-with-latex-preview t) to init.el.
;; To customize the preview, set org-format-latex-options.
;; Note that this requires installing Latex. For MacOS, install MacTex with brew install --cask mactex.

;; To enable preview on startup globally
(setq org-startup-with-latex-preview nil)

;; Slightly increase the size of the Latex fragment previews.
;; (setq org-format-latex-options (plist-put org-format-latex-options :scale 1.2))

;; Put the generated image to an absolute directory.
;; A side effect is that all latex images from this shared directory
;; might be copied during exporting.
;; e.g., ox-hugo (https://github.com/kaushalmodi/ox-hugo/issues/723)
(setq org-preview-latex-image-directory
      (expand-file-name "preview/ltximg/" user-emacs-directory))

;;;;; write-window-setup

;; 마진 구성이 필요할 수도 있다.
;; (defun my/markdown-setup ()
;;   (interactive)
;;   ;; (visual-line-mode 1)
;;   ;; (setq buffer-face-mode-face '(:family "iA Writer Quattro S" :height 200 :foreground "#aba7a0"))
;;   ;; (buffer-face-mode)
;;   ;; (setq line-spacing 3)
;;   (setq left-margin-width 8)
;;   (setq right-margin-width 8)
;;   (flyspell-mode 1)
;;   (setq global-hl-line-mode nil)
;;   (setq header-line-format " ")
;;   )
;; (add-hook 'markdown-mode-hook #'my/markdown-setup)

;; Writing Window Setup on Dired
;; 괜찮다. 화면 버퍼 구성이 여러모로 집중하기 좋다.
(defun write-window-setup ()
  (interactive)
  (split-window-right)
  (windmove-right)
  (split-window-below)
  (windmove-left)
  (find-file "*draft.org" t)
  (windmove-right)
  (find-file "*notes.txt" t) ; txt
  (windmove-left))
(with-eval-after-load 'dired
  (define-key dired-mode-map [f3] #'write-window-setup))

;;;;; ox-hugo

(with-eval-after-load 'ox-hugo
  (setq org-hugo-auto-set-lastmod t
        org-hugo-suppress-lastmod-period 43200.0)

  (setq org-hugo-front-matter-format 'yaml)

  (setq org-hugo-section "post")
  ;; (setq org-hugo-paired-shortcodes "hint details mermaid sidenote")

  ;; https://ox-hugo.scripter.co/doc/formatting/
  ;; if org-hugo-use-code-for-kbd is non-nil
  ;; Requires CSS to render the <kbd> tag as something special.
  ;; eg: ~kbd~
  (setq org-hugo-use-code-for-kbd t)

  ;; https://ox-hugo.scripter.co/doc/linking-numbered-elements/
  ;; org-export-dictionary 에 Figure, Table 에 한글 번역을 넣으면
  ;; 한글로 바뀌어 export 될 것이다.
  (setq org-hugo-link-desc-insert-type t)

  ;; Assume all static files are images for now otherwise this
  ;; defaults to /ox-hugo/mypicture.png which is ugly
  (setq org-hugo-default-static-subdirectory-for-externals "images") ; imgs

  ;; Override the default `org-hugo-export-creator-string' so that this
  ;; string is consistent in all ox-hugo tests.
  (setq org-hugo-export-creator-string "Emacs + Org mode + ox-hugo")

  ;; In that normal example of the sidenote, ox-hugo trims the whitespace around
  ;; the sidenote block. That is configured by customizing the
  ;; org-hugo-special-block-type-properties variable:
  ;; (add-to-list 'org-hugo-special-block-type-properties '("sidenote" . (:trim-pre t :trim-post t)))

  ;; If this property is set to an empty string, this heading will not be auto-inserted.
  ;; default value is 'References'
  ;; https://ox-hugo.scripter.co/doc/org-cite-citations/
  (plist-put org-hugo-citations-plist :bibliography-section-heading "References")
  )

;;;;; DONT display-line-number org-mode and markdown-mode

;; (unless *is-termux*
;;   (add-hook 'org-mode-hook 'display-line-numbers-mode)
;;   (add-hook 'markdown-mode-hook 'display-line-numbers-mode)
;;   )

;;;;; DONT Jekyll Blogging

;; 이미지 아웃풋


;;;;;; my/jekyll-insert-post-url / my/jekyll-insert-image-url

;; /home/junghan/sync/man/dotsamples/vanilla/douo-dotfiles-kitty/init.el
;; (defun my/jekyll-insert-image-url ()
;;   (interactive)
;;   (let* ((files (directory-files "../assets/images"))
;;           (selected-file (completing-read "Select image: " files nil t)))
;;     (insert (format "![%s](/assets/images/%s)" selected-file selected-file))))

;; (defun my/jekyll-insert-post-url ()
;;   (interactive)
;;   (let* ((files (remove "." (mapcar #'file-name-sans-extension (directory-files "."))))
;;           (selected-file (completing-read "Select article: " files nil t)))
;;     (insert (format "{%% post_url %s %%}" selected-file))))

;; (defun jekyll-default-image ()
;;   (interactive)
;;   (let ((name (format "{{ site.asseturl }}/%s-00.jpg"
;;                 (file-name-base (buffer-file-name)))))
;;     (kill-new name)
;;     (message "Copied default image name '%s' to the clipboard." name)))

;;;;;; TODO jekyll  blogging

;; hrs-dotfiles/emacs/.config/emacs/configuration.org:1883

;; I maintain a blog written in Jekyll. There are plenty of command-line tools to automate creating a new post, but staying in my editor minimizes friction and encourages me to write.

;; 저는 지킬로 작성된 블로그를 운영하고 있습니다. 새 글 작성을 자동화할 수 있는
;; 명령줄 도구는 많지만, 에디터에 머무르면 마찰을 최소화하고 글쓰기에 더 집중할 수
;; 있습니다.

;; This defines a =+new-blog-post= function, which prompts the user for a title and creates a new draft (with a slugged file name) in the blog's =_drafts/= directory. The new post includes appropriate YAML header information.

;; 이 함수는 사용자에게 제목을 묻는 메시지를 표시하고 블로그의 =_drafts/=
;; 디렉터리에 새 글(슬러그 파일 이름 포함)을 작성하는 =+new-blog-post= 함수를
;; 정의합니다. 새 글에는 적절한 YAML 헤더 정보가 포함됩니다.

;; This also defines =+publish-post= and =+unpublish-post=, which adjust the date in the YAML front matter and rename the file appropriately.

;; 또한 =+publish-post= 및 =+unpublish-post=를 정의하여 YAML 앞부분의 날짜를
;; 조정하고 파일 이름을 적절하게 변경합니다.

;; (defvar +jekyll-drafts-directory (expand-file-name "~/git/blog/_drafts/"))
;; (defvar +jekyll-posts-directory (expand-file-name "~/git/blog/_posts/"))
;; (defvar +jekyll-post-extension ".md")

;; (defun +timestamp ()
;;   (format-time-string "%Y-%m-%d"))

;; (defun +replace-whitespace-with-hyphens (s)
;;   (replace-regexp-in-string " " "-" s))

;; (defun +replace-nonalphanumeric-with-whitespace (s)
;;   (replace-regexp-in-string "[^A-Za-z0-9 ]" " " s))

;; (defun +remove-quotes (s)
;;   (replace-regexp-in-string "[\'\"]" "" s))

;; (defun +replace-unusual-characters (title)
;;   "Remove quotes, downcase everything, and replace characters
;; that aren't alphanumeric with hyphens."
;;   (+replace-whitespace-with-hyphens
;;     (s-trim
;;       (downcase
;;         (+replace-nonalphanumeric-with-whitespace
;;           (+remove-quotes title))))))

;; (defun +slug-for (title)
;;   "Given a blog post title, return a convenient URL slug.
;;    Downcase letters and remove special characters."
;;   (let ((slug (+replace-unusual-characters title)))
;;     (while (string-match "--" slug)
;;       (setq slug (replace-regexp-in-string "--" "-" slug)))
;;     slug))

;; (defun +jekyll-yaml-template (title)
;;   "Return the YAML header information appropriate for a blog
;;    post. Include the title, the current date, the post layout,
;;    and an empty list of tags."
;;   (concat
;;     "---\n"
;;     "title: " title "\n"
;;     "date:\n"
;;     "layout: post\n"
;;     "# mathjax: true\n"
;;     "# pdf_file: " (+slug-for title) ".pdf\n"
;;     "tags: []\n"
;;     "---\n\n"))

;; (defun +new-blog-post (title)
;;   "Create a new blog draft in Jekyll."
;;   (interactive "sPost title: ")
;;   (let ((post (concat +jekyll-drafts-directory
;;                 (+slug-for title)
;;                 +jekyll-post-extension)))
;;     (if (file-exists-p post)
;;       (find-file post)
;;       (find-file post)
;;       (insert (+jekyll-yaml-template title)))))

;; (defun +jekyll-draft-p ()
;;   "Return true if the current buffer is a draft."
;;   (equal
;;     (file-name-directory (buffer-file-name (current-buffer)))
;;     +jekyll-drafts-directory))

;; (defun +jekyll-published-p ()
;;   "Return true if the current buffer is a published post."
;;   (equal
;;     (file-name-directory (buffer-file-name (current-buffer)))
;;     +jekyll-posts-directory))

;; (defun +publish-post ()
;;   "Move a draft post to the posts directory, rename it to include
;; the date, reopen the new file, and insert the date in the YAML
;; front matter."
;;   (interactive)
;;   (cond ((not (+jekyll-draft-p))
;;           (message "This is not a draft post."))
;;     ((buffer-modified-p)
;;       (message "Can't publish post; buffer has modifications."))
;;     (t
;;       (let ((filename
;;               (concat +jekyll-posts-directory
;;                 (+timestamp) "-"
;;                 (file-name-nondirectory
;;                   (buffer-file-name (current-buffer)))))
;;              (old-point (point)))
;;         (rename-file (buffer-file-name (current-buffer))
;;           filename)
;;         (kill-buffer nil)
;;         (find-file filename)
;;         (set-window-point (selected-window) old-point)
;;         (save-excursion
;;           (beginning-of-buffer)
;;           (replace-regexp "^date:$" (concat "date: " (+timestamp))))
;;         (save-buffer)
;;         (message "Published post!")))))

;; (defun +unpublish-post ()
;;   "Move a published post to the drafts directory, rename it to
;; exclude the date, reopen the new file, and remove the date in the
;; YAML front matter."
;;   (interactive)
;;   (cond ((not (+jekyll-published-p))
;;           (message "This is not a published post."))
;;     ((buffer-modified-p)
;;       (message "Can't publish post; buffer has modifications."))
;;     (t
;;       (let ((filename
;;               (concat +jekyll-drafts-directory
;;                 (substring
;;                   (file-name-nondirectory
;;                     (buffer-file-name (current-buffer)))
;;                   11 nil)))
;;              (old-point (point)))
;;         (rename-file (buffer-file-name (current-buffer))
;;           filename)
;;         (kill-buffer nil)
;;         (find-file filename)
;;         (set-window-point (selected-window) old-point)
;;         (save-excursion
;;           (beginning-of-buffer)
;;           (replace-regexp "^date: [0-9][0-9][0-9][0-9]-[0-9][0-9]-[0-9][0-9]$" "date:"))
;;         (save-buffer)
;;         (message "Returned post to drafts!")))))

;; (defun +tags-from-tag-line (line)
;;   "Given a line of tags from a blog post (like \"tags: [animals, design, cephalopods]\") return a sorted list of the tags (like '(\"animals\" \"cephalopods\" \"design\"))."
;;   (sort (mapcar #'string-trim
;;           (-> (string-trim line)
;;             (substring 7 -1)
;;             (split-string ",")))
;;     #'string<))

;; (defun +tag-lines ()
;;   "Return all the lines of tags from all existing blog posts."
;;   (seq-remove #'string-empty-p
;;     (split-string
;;       (shell-command-to-string
;;         (format "grep --no-filename \"^tags: \\[.*\\]$\" %s"
;;           (concat (file-name-as-directory +jekyll-posts-directory) "*")))
;;       "\n")))

;; (defun +existing-blog-tags ()
;;   "Return a sorted list of all the tags used in my blog posts."
;;   (-> (mapcar #'+tags-from-tag-line (+tag-lines))
;;     (flatten-list)
;;     (seq-uniq)
;;     (sort #'string<)))

;; (defun +insert-blog-tag ()
;;   "Prompt for one of the existing tags used in the blog and insert
;; it in the YAML front matter appropriately."
;;   (interactive)
;;   (save-excursion
;;     (beginning-of-buffer)
;;     (search-forward-regexp "^tags: \\[")
;;     (insert
;;       (completing-read "Insert tag: " (+existing-blog-tags))
;;       (if (looking-at "\\]") "" ", ")))
;;   (message "Tagged!"))

;;;;; Zettelkasten - Denote

;;;;;; cape mode setup for completion-at-point

;; Default nil
(setq-default completion-at-point-functions nil)

;; (advice-add 'eglot-completion-at-point :around #'cape-wrap-buster)

;; Add `completion-at-point-functions', used by `completion-at-point'.
(defun cape-text-mode-setup ()
  (interactive)
  (add-to-list 'completion-at-point-functions #'cape-dict)
  (add-to-list 'completion-at-point-functions #'cape-file)
  ;; (add-to-list 'completion-at-point-functions #'cape-history)
  (add-to-list 'completion-at-point-functions #'cape-dabbrev) ; top
  )

;; 2023-07-08 순서 때문에 따로 확실하게 점검한다.
(defun cape-markdown-mode-setup ()
  (interactive)
  (add-to-list 'completion-at-point-functions #'cape-dict)
  (add-to-list 'completion-at-point-functions #'cape-file)
  ;; (add-to-list 'completion-at-point-functions #'cape-history)
  (add-to-list 'completion-at-point-functions #'cape-dabbrev) ; top
  )

(defun cape-org-mode-setup ()
  ;; (add-to-list 'completion-at-point-functions #'cape-elisp-block)
  ;; (add-to-list 'completion-at-point-functions #'cape-history)
  ;; (add-to-list 'completion-at-point-functions #'cape-dict)
  (add-to-list 'completion-at-point-functions #'cape-file)
  (add-to-list 'completion-at-point-functions #'cape-dabbrev)
  ;; (add-to-list 'completion-at-point-functions #'zk-completion-at-point) ;; top
  )

(defun cape-prog-mode-setup ()
  ;; (add-to-list 'completion-at-point-functions #'cape-file)
  (add-to-list 'completion-at-point-functions #'cape-dabbrev)
  ;; (add-to-list 'completion-at-point-functions #'cape-history)
  ;; (add-to-list 'completion-at-point-functions #'cape-keyword) ;; no.1
  )

(add-hook 'markdown-mode-hook 'cape-markdown-mode-setup)
(add-hook 'org-mode-hook 'cape-org-mode-setup)
;; (add-hook 'conf-mode-hook 'cape-prog-mode-setup)

(add-hook 'prog-mode-hook 'cape-prog-mode-setup)
(remove-hook 'org-mode-hook #'org-eldoc-load)

;; In non-programming-buffers, we don't want `pcomplete-completions-at-point'
;; or 't' which seems to complete everything.
;; (defun ash/fix-completion-for-nonprog-buffers ()
;;   (setq completion-at-point-functions
;;         (-remove-item t (append (-remove-item #'pcomplete-completions-at-point completion-at-point-functions)
;;                                 '(cape-file cape-abbrev cape-rfc1345)))))
;; (add-hook 'org-mode-hook #'ash/fix-completion-for-nonprog-buffers)
;; (add-hook 'notmuch-message-mode-hook #'ash/fix-completion-for-nonprog-buffers)

;;;;;; TODO link-hint-aw

;; (:map gr-map
;;       ("o" . link-hint-aw-select))
;; (add-to-list 'link-hint-aw-select-ignored-buffers 'org-side-tree-mode)
;; (add-to-list 'link-hint-aw-select-ignored-buffers 'zk-index-mode)
;; open org-links in same window
;; allows link-hint--aw-select-org-link to work properly
;; (with-eval-after-load 'org
;;   (setf (cdr (assoc 'file org-link-frame-setup)) 'find-file)))

;;;;;; TODO denote-region with org-structure-template

;; (defun my-denote-region-org-structure-template (_beg _end)
;;   (when (derived-mode-p 'org-mode)
;;     (activate-mark)
;;     (call-interactively 'org-insert-structure-template)))
;; (add-hook 'denote-region-after-new-note-functions #'my-denote-region-org-structure-template)

;;;;; DONT Flyspell Jinx-

;; (add-hook 'text-mode-hook #'flyspell-mode) ; hangul
;; (add-hook 'prog-mode-hook #'flyspell-prog-mode)

;; (add-hook 'prog-mode-hook #'jinx-mode) ; english

;;;;; DONT capture-templates : Taste Project Somday Maybe Log

;; (add-to-list 'org-capture-templates
;;              '("w" "Review: Weekly Review" entry (file+datetree ,(concat org-workflow-directory "reviews.org"))
;;                (file ,(concat org-directory "templates/weeklyreviewtemplate.org")))
;;              )
;; (setq org-capture-templates
;;       '(("t" "Task" entry (file "~/org/inbox.org")
;;          "* TODO %?\n")
;;         ("p" "Project" entry (file+headline "~/org/todo.org" "Projects")
;;          (file "~/org/templates/newprojecttemplate.org"))
;;         ("s" "Someday" entry (file+headline "~/org/somedaymaybe.org" "Someday / Maybe")
;;          "* SOMEDAY %?\n")
;;         ("m" "Maybe" entry (file+headline "~/org/somedaymaybe.org" "Someday / Maybe")
;;          "* MAYBE %?\n")
;;         ("l" "Log" entry (file+olp+datetree "~/org/log.org" "Log")
;;          (file "~/org/templates/logtemplate.org"))))

;;;;; DONT org-clock-budget

;; https://github.com/Fuco1/org-clock-budget/tree/master

;;'(org-clock-budget-default-sort-column '("BUDGET_WEEK" ratio desc))
;; '(org-clock-budget-intervals
;;   '(("BUDGET_YEAR" org-clock-budget-interval-this-year)
;;     ("BUDGET_MONTH" org-clock-budget-interval-this-month)
;;     ("BUDGET_WEEK" org-clock-budget-interval-this-week)))
;; '(org-clock-budget-ratio-faces
;;   '((1.0 font-lock-warning-face)
;;     (0.9 font-lock-variable-name-face)
;;     (0.0 font-lock-keyword-face)))


;;;;; DONT org-noter

;; (use-package org-pdftools
;;   :hook (org-mode . org-pdftools-setup-link))

;; (use-package org-noter
;;   :after (:any org pdf-view)
;;   :config
;;   (setq org-noter-always-create-frame nil
;;     ;; org-noter-notes-window-location 'other-frame
;;     org-noter-hide-other nil
;;     org-noter-insert-note-no-questions t
;;     org-noter-separate-notes-from-heading t
;;     ;; org-noter-notes-search-path (list org_roam_dir)
;;     org-noter-notes-search-path '("~/sync/org/")
;;     org-noter-auto-save-last-location t))

;; (use-package org-noter
;;   :defer 3
;;   :after org
;;   :config
;;   (setq org-noter-always-create-frame nil
;;     org-noter-kill-frame-at-session-end nil))

;; (use-package org-noter-pdftools
;;   :after org-noter
;;   :config
;;   (with-eval-after-load 'pdf-annot
;;     (add-hook 'pdf-annot-activate-handler-functions #'org-noter-pdftools-jump-to-note)))


;;;;; org-copy-link-at-point

;; 커서가 위치한 org element 에 link 프로퍼티가 있으면 클립보드로 복사하는 함수.
;; org mode 에서 =SPC m l y= 키에 바인딩.
(defun my/org-copy-link-at-point ()
  (interactive)
  (let ((link (org-element-property :raw-link (org-element-context))))
    (when link
      (kill-new link))))
```


#### <span class="section-num">4.1.3</span> 'Coding' - prog-mode {#h:47d43492-0e3e-4986-9ac3-df4328510944}

```elisp
;;;; 'Coding' - prog-mode

;;;;; minor stuff

(defun ash/strdec-to-hex (n)
  "Given a decimal as a string, convert to hex.
This has to be done as a string to handle 64-bit or larger ints."
  (concat "0x" (replace-regexp-in-string "16#" "" (calc-eval `(,n calc-number-radix 16)))))

;;;;; TODO batteries-inclued-with emcas

;; from from https://karthinks.com/software/batteries-included-with-emacs/

;;;;; ws-butler-mode

;; (ws-butler-keep-whitespace-before-point nil)
;; (ws-butler-global-exempt-modes '(special-mode comint-mode term-mode eshell-mode diff-mode markdown-mode))
(add-hook 'prog-mode-hook 'ws-butler-mode)

;;;;; DONT org-protocol-capture-html

;; (require 'org-protocol-capture-html)

;; ;; (setq org-protocol-default-template-key "w")
;; (add-to-list 'org-capture-templates
;;   '("w" "Web site"
;;      entry (file+olp org-refile-file "Web")
;;      "* %c :website:\n%U %?%:initial")
;;   )
;;;;; save-macro
;; Save a recorded macro with a name
(defun save-macro (name)
  "Takes a name as argument and save the last defined macro under
   this name at the end of your .emacs"
  (interactive "SName of the macro :")  ; ask for the name of the
                                        ; macro
  (kmacro-name-last-macro name)         ; use this name for
                                        ; the macro
  (find-file user-init-file)            ; open ~/.emacs
                                        ; or other user init file
  (goto-char (point-max))               ; go to
                                        ; the end of the .emacs
  (newline)                             ; insert a newline
  (insert-kbd-macro name)               ; copy the macro
  (newline)                             ; insert a newline
  (switch-to-buffer nil))               ; return to the initial buffer

;;;;; deepl api-key with go-translate

(require 'posframe)
(require 'go-translate)

(setq gts-default-translator
      (gts-translator :picker
                      (gts-prompt-picker)
                      ;;(gts-noprompt-picker)
                      ;;(gts-noprompt-picker :texter (gts-whole-buffer-texter))

                      :engines
                      (list
                       ;; (gts-bing-engine)
                       ;; (gts-google-engine)
                       (gts-google-engine :parser (gts-google-summary-parser))
                       ;; (gts-google-engine :parser (gts-google-parser))
                       (gts-deepl-engine :auth-key my_deepl_apikey :pro t)
                       ;;(gts-google-rpc-engine :parser (gts-google-rpc-summary-parser))
                       )

                      :render
                      (gts-buffer-render)

                      ;;(gts-posframe-pop-render)
                      ;;(gts-posframe-pop-render :backcolor "#333333" :forecolor "#ffffff")
                      ;;(gts-posframe-pin-render)
                      ;;(gts-posframe-pin-render :position (cons 1200 20))
                      ;;(gts-posframe-pin-render :width 80 :height 25 :position (cons 1000 20) :forecolor "#ffffff" :backcolor "#111111")
                      ;;(gts-kill-ring-render)

                      :splitter nil
                      ;; (gts-paragraph-splitter)
                      ))

(setq gts-deepl-langs-mapping '(("en" . "EN")
                                ("zh" . "ZH")
                                ("ko" . "KO") ; Korean
                                ("de" . "DE") ; German
                                ("fr" . "FR") ; French
                                ("it" . "IT") ; Italian
                                ("ja" . "JA") ; Japanese
                                ("es" . "ES") ; Spanish
                                ("nl" . "NL") ; Dutch
                                ("pl" . "PL") ; Polish
                                ("pt" . "PT") ; Portuguese (all Portuguese varieties mixed)
                                ("ru" . "RU") ; Russian
                                ))

;;;;; magit

;; Set locations of all your Git repositories
;; with a number to define how many sub-directories to search
;; `SPC g L' - list all Git repositories in the defined paths,
(setq magit-repository-directories
      '(
        ("~/spacemacs/" . 0)
        ("~/.spacemacs.d/" . 0)
        ("~/git/" . 1)
        ("~/mydotfiles/" . 0)
        ;; ("~/sync/code/" . 2)
        ))

;;;;; forge

;; Configure number of topics show, open and closed
;; use negative number to toggle the view of closed topics
;; using `SPC SPC forge-toggle-closed-visibility'
(setq  forge-topic-list-limit '(100 . -10))
;; set closed to 0 to never show closed issues
;; (setq  forge-topic-list-limit '(100 . 0))

;; GitHub user and organization accounts owned
;; used by @ c f  to create a fork
(setq forge-owned-accounts
      '(("junghan0611" "junghanacs")))

;;;;; DONT link-preview

;; https://www.peekalink.io/
;; (require 'link-preview)
;; (spacemacs/set-leader-keys "ip" 'link-preview-insert)

;;;;; TODO immersive-translate

;; (add-to-list 'load-path "~/sync/emacs/emacs-pkgs/emacs-immersive-translate")
(require 'immersive-translate)
(setq immersive-translate-backend 'deepl)
(setq immersive-translate-deepl-api "https://api.deepl.com/v2/translate")
;; (setq immersive-translate-deepl-source-language "EN")
;; (setq immersive-translate-deepl-target-language "KO")
(setq immersive-translate-curl-get-translation-alist '(deepl . immersive-translate-curl-deepl-get-translation))
(setq immersive-translate-curl-get-args-alist '(deepl . immersive-translate-deepl-get-args))

(setq immersive-translate-auto-idle 2.0) ; default 0.5

;; (setq immersive-translate-backend 'chatgpt
;;   immersive-translate-chatgpt-host "api.openai.com")
;; (setq immersive-translate-chatgpt-user-prompt "You will be provided with text delimited by triple backticks, your task is to translate the wrapped text into Korean. You should only output the translated text. \n```%s```")
;; (setq immersive-translate-chatgpt-model "gpt-3.5-turbo")

;; (add-hook 'elfeed-show-mode-hook #'immersive-translate-setup)
;; (add-hook 'nov-mode-hook #'immersive-translate-setup)
;; (add-hook 'Info-mode-hook #'immersive-translate-setup)
(add-hook 'help-mode-hook #'immersive-translate-setup)
(add-hook 'helpful-mode-hook #'immersive-translate-setup)

;;;;; Toggle Window Layout

;; 윈도우를 두개로 나누었을때 가로, 세로 나누기로 변경하는 함수.
;; SPC w +

;;;;; goto-last-change

;; goto-chg 패키지에서 아래 함수 제공
;; evil-goto-last-change-reverse (g ,)
;; evil-goto-last-change (g ;)

;;;###autoload
(defun my/goto-last-change ()
  (interactive)
  (outline-show-all) ; 전체를 펼치고 찾아라!
  (goto-last-change))
(global-set-key (kbd "C-x ,") 'my/goto-last-change)

;;;;; TODO lsp formatter with apheleaa

;; lsp 포메터 앞단에 apheleia 를 적용하는 방법인데, 이게 같은 키로 포메터만
;; 바꾸는 좋은 방법인가?!

;; ASOK spacemacs
;; (with-eval-after-load 'apheleia
;;   (cl-defun asok/lsp-format-buffer-formatter (&key buffer scratch callback &allow-other-keys)
;;     ;; `lsp-format-buffer' requires `buffer-file-name' to be set.
;;     (let ((buffer-file-name (buffer-file-name buffer)))
;;       (with-lsp-workspaces (with-current-buffer buffer (lsp-workspaces))
;;         (with-current-buffer scratch
;;           (lsp-format-buffer))))
;;     (when callback (funcall callback)))

;;   ;; add-to-list will place the new element in the front so it will shadow the
;;   ;; default formatter for ruby-mode
;;   ;; (add-to-list 'apheleia-mode-alist '(python-mode . lsp-format-buffer-formatter)) ; TODO for test
;;   ;; (add-to-list 'apheleia-formatters '(lsp-format-buffer-formatter . asok/lsp-format-buffer-formatter))

;;   )

;;;;; DONT Built-in string-edit

;; only available in 29 or higher
;; (use-package string-edit
;;   :ensure nil
;;   :init
;;   (defun gopar/replace-str-at-point (new-str)
;;     (let ((bounds (bounds-of-thing-at-point 'string)))
;;       (when bounds
;;         (delete-region (car bounds) (cdr bounds))
;;         (insert new-str))))

;;   (defun gopar/edit-string-at-point ()
;;     (interactive)
;;     (let ((string (thing-at-point 'string t)))
;;       (string-edit "String at point:" string 'gopar/replace-str-at-point :abort-callback (lambda ()
;;                      (exit-recursive-edit)
;;                      (message "Aborted edit"))))))



;;;;; emacspeak

(when *run-emacspeak*
  (require 'my-emacspeak))

;;;;; DONT spookfox for firefox

;; [[https://sachachua.com/blog/2023/01/using-spookfox-to-scroll-firefox-up-and-down-from-emacs/][Using Spookfox to scroll Firefox up and down from Emacs :: Sacha Chua]]
;; 와. 이거 물건이다. 완전 편하다.
;; Shift+Super+Return= :: open firefox new tab (i3)
;; 아주 유연하게 연결이 된다. 스크랩 기능도 편하다. 이거 필요했다.

;; (use-package spookfox
;;   ;; :if window-system
;;   :if (not (or my/remote-server *is-termux*))
;;   :config
;;   (require 'spookfox-tabs)
;;   ;; (require 'spookfox-org-tabs)
;;   (require 'spookfox-js-injection)

;;   (add-to-list 'spookfox-enabled-apps 'spookfox-tabs)
;;   ;; (add-to-list 'spookfox-enabled-apps 'spookfox-org-tabs)
;;   (add-to-list 'spookfox-enabled-apps 'spookfox-js-injection)

;;   ;; 탭은 안쓴다.
;;   ;; (setq spookfox-saved-tabs-target
;;   ;;       `(file+headline ,(expand-file-name "spookfox.org" "~/sync/org/workflow/") "Tabs"))
;;   ;; (spookfox-init) ; manually enable
;;   )

;; (defun spookfox-start () (interactive) (spookfox-init))

;; (spacemacs/set-leader-keys "os" 'spookfox-start)

;; (defun my-spookfox-scroll-down ()
;;   (interactive)
;;   (spookfox-eval-js-in-active-tab "window.scrollBy(0, document.documentElement.clientHeight);"))

;; (defun my-spookfox-scroll-up ()
;;   (interactive)
;;   (spookfox-eval-js-in-active-tab "window.scrollBy(0, -document.documentElement.clientHeight);"))

;; (global-set-key (kbd "C-s-j") 'my-spookfox-scroll-down)
;; (global-set-key (kbd "C-s-k") 'my-spookfox-scroll-up)
;; ;; (global-set-key (kbd "C-s-n") 'my-spookfox-scroll-down)
;; ;; (global-set-key (kbd "C-s-p") 'my-spookfox-scroll-up)

;; ;; This code opens a tab without switching keyboard focus away from Emacs:
;; (defun my-spookfox-background-tab (url &rest args)
;;   "Open URL as a background tab."
;;   (if spookfox--connected-clients
;;     (spookfox-tabs--request (cl-first spookfox--connected-clients) "OPEN_TAB" `(:url ,url))
;;     (browse-url url)))

;; (defun my-spookfox-insert-link-from-page ()
;;   (interactive)
;;   (let* ((links (my-spookfox-get-links))
;;           (link (completing-read
;;                   "Link: "
;;                   (my-presorted-completion-table
;;                     links))))
;;     (insert (org-link-make-string link (my-page-title link)))))

;;;;; exercism

;; for emacs-lisp-mode
(defun ert/eval-and-run-all-tests-in-buffer ()
  "Deletes all loaded tests from the runtime, evaluates the current buffer and runs all loaded tests with ert."
  (interactive)
  (ert-delete-all-tests)
  (eval-buffer)
  (ert 't))

;; pytest.ini
;; [pytest]
;; markers =
;;     task: A concept exercise task.
(with-eval-after-load 'pytest
  (add-to-list 'pytest-project-root-files "pytest.ini"))

;;;;; DONT revert-buffer-all

;; (require 'revert-buffer-all)
;; (global-set-key (kbd "C-S-M-r") 'revert-buffer-all)

;;;;; treesit-er-expansions

(require 'treesit-er-expansions)

;;;;; TODO docsim

;; 한글 튜닝 필요
;; https://github.com/hrs/docsim.el

;; (use-package org-wc
;;   :after org
;;   :defer 1
;;   :bind
;;   (:map gr-map
;;         ("W" . org-wc-display)))

;; (use-package docsim
;;     :defer t
;;     :after zk
;;     :commands (docsim-search
;;                 docsim-search-buffer)
;;     :config

;;     ;;   (setq docsim-assume-english nil)

;;     (setq docsim-search-paths (list zk-directory))
;;     (setq docsim-get-title-function 'gr/docsim--get-title-function-zk)

;;     (defun gr/docsim--get-title-function-zk (path)
;;       "Return a title determined by parsing the file at PATH."
;;       (if (zk-file-p path)
;;         (zk--parse-file 'title path)
;;         path))

;;     (defun gr/docsim-search (query)
;;       "Search for notes similar to QUERY.

;; This calls out to the external `docsim' tool to perform textual
;; analysis on all the notes in `docsim-search-paths', score them by
;; similarity to QUERY, and return the sorted results, best first.

;; Include the similarity scores (between 0.0 and 1.0) of each note
;; if `docsim-show-scores' is non-nil.

;; Show at most `docsim-limit' results (or all of them, if
;;                                         `docsim-limit' is nil)."
;;       (interactive (list (docsim--read-search-term)))
;;       (let* ((results (docsim--query query))
;;               (files (mapcar 'car results)))
;;         (find-file
;;           (funcall zk-select-file-function
;;             "Similar Notes:"
;;             results
;;             'zk-docsim--group
;;             'identity))))


;;     (defun zk-docsim ()
;;       "Find notes similar to current buffer using docsim."
;;       (interactive)
;;       (gr/docsim-search (current-buffer)))
;;     )

;;;;; Tune for prog-mode

;; web-mode && react layer
(setq-default
 css-indent-offset 4
 web-mode-markup-indent-offset 4
 web-mode-css-indent-offset 4
 web-mode-code-indent-offset 4
 web-mode-attr-indent-offset 4)

;; Color the string of whatever color code they are holding
(add-hook 'prog-mode-hook 'rainbow-mode) ; 2023-11-23 on
(add-hook 'prog-mode-hook 'global-flycheck-mode) ; 2023-11-30 on

;;;;; flycheck hook

(with-eval-after-load 'flycheck
  (setq flycheck-help-echo-function nil)
  (setq flycheck-display-errors-function nil)
  )

;; SAMPLE
;; (use-package flycheck
;;   :diminish flycheck-mode
;;   :hook (prog-mode . global-flycheck-mode)
;;   :custom
;;   (flycheck-check-syntax-automatically '(save idle-change new-line idle-buffer-switch mode-enabled))
;;   (flycheck-disabled-checkers '(emacs-lisp emacs-lisp-checkdoc javascript-jshint))
;;   (flycheck-javascript-eslint-executable "eslint_d")
;;   (flycheck-global-modes '(not rust-ts-mode))
;;   :config
;;   (flycheck-add-mode 'javascript-eslint 'js-ts-mode)
;;   (flycheck-add-mode 'javascript-eslint 'typescript-ts-mode)
;;   (flycheck-add-mode 'javascript-eslint 'tsx-ts-mode))

;;;;; DONT dap-mode

;; (unless *is-termux*
;;   (require 'dap-chrome)
;;   ;; (add-hook 'lsp-mode-hook #'lsp-enable-which-key-integration)
;;   ;; (require 'dap-firefox)
;;   ;; (setq dap-firefox-debug-program
;;   ;;   '("node"
;;   ;;      "~/.spacemacs.d/.extension/vscode/firefox-devtools.vscode-firefox-debug/extension/dist/adapter.bundle.js"))
;;   ;; default
;;   (setq dap-auto-configure-features '(sessions locals breakpoints expressions controls tooltip))
;;   (dap-auto-configure-mode t)
;;   )
;;  (("<f7>" . dap-step-in)
;;   ("<f8>" . dap-next)
;;   ("<f9>" . dap-continue)

;;;;; Tree-sitter/treesit

;; required. Assumes that the modules are in your emacs directory, inside
;; modules build from https://github.com/casouri/tree-sitter-module
;; - syntax highlighting
;; - textobjects
;; - folding
;; https://emacsconf.org/2022/talks/treesitter/
;; modules build from https://github.com/casouri/tree-sitter-module
;; (setq treesit-extra-load-path '(concat (user-emacs-directory) "tree-sitter-module/dist" ))

(require 'treesit)
(setq treesit-font-lock-level 4) ; default 3

;; ;; M-x treesit-install-language-grammar
(setq treesit-language-source-alist
      '((awk "https://github.com/Beaglefoot/tree-sitter-awk")

        ;; (javascript "https://github.com/tree-sitter/tree-sitter-javascript" "master" "src")
        ;; (bash "https://github.com/tree-sitter/tree-sitter-bash")
        ;; (yaml "https://github.com/ikatyang/tree-sitter-yaml")
        ;; (cmake "https://github.com/uyha/tree-sitter-cmake")
        ;; (css "https://github.com/tree-sitter/tree-sitter-css")
        ;; (elisp "https://github.com/Wilfred/tree-sitter-elisp")
        ;; (go "https://github.com/tree-sitter/tree-sitter-go")
        ;; (html "https://github.com/tree-sitter/tree-sitter-html")
        ;; (json "https://github.com/tree-sitter/tree-sitter-json")
        ;; (make "https://github.com/alemuller/tree-sitter-make")
        ;; (markdown "https://github.com/ikatyang/tree-sitter-markdown")
        ;; (python "https://github.com/tree-sitter/tree-sitter-python")
        ;; (toml "https://github.com/tree-sitter/tree-sitter-toml")
        ;; (tsx "https://github.com/tree-sitter/tree-sitter-typescript" "master" "tsx/src")
        ;; (typescript "https://github.com/tree-sitter/tree-sitter-typescript" "master" "typescript/src")
        ))

;; enable ts-mode as default
(push '(awk-mode . awk-ts-mode) major-mode-remap-alist)
;; (push '(js2-mode . js-ts-mode) major-mode-remap-alist)

;; 필요 없다. evil-textobject-tree-sitter 가 매핑한다.
;; 2023-11-03 enable for elixir
;; (push '(elixir-mode . elixir-ts-mode) major-mode-remap-alist)
;; (push '(bash-mode . bash-ts-mode) major-mode-remap-alist)
;; (push '(yaml-mode . yaml-ts-mode) major-mode-remap-alist)

;; (push '(python-mode . python-ts-mode) major-mode-remap-alist)
;; (push '(css-mode . css-ts-mode) major-mode-remap-alist)
;; (push '(javascript-mode . js-ts-mode) major-mode-remap-alist)
;; (push '(json-mode . json-ts-mode) major-mode-remap-alist)
;; (push '(js-json-mode . json-ts-mode) major-mode-remap-alist)
;; (push '(typescript-mode . tsx-ts-mode) major-mode-remap-alist)
;; (add-to-list 'auto-mode-alist '("\\.tsx?\\'" . tsx-ts-mode))

(use-package eglot
  :ensure nil
  :config
  ;; (add-to-list 'eglot-server-programs '(elixir-ts-mode "language_server.sh"))
  (advice-add 'eglot-completion-at-point :around #'cape-wrap-buster)
  )


;;;;; consult custom

(with-eval-after-load 'consult
  (define-key minibuffer-local-map (kbd "M-s") 'consult-history)
  (define-key minibuffer-local-map (kbd "M-r") 'consult-history)

  (setq consult-project-function (lambda (_) (projectile-project-root)))
  (define-key projectile-command-map (kbd "b") 'consult-project-buffer)
  )

;;;;; DONT Elixir

;; manually use on lsp mode
;; (remove-hook 'elixir-mode-local-vars-hook 'spacemacs//elixir-setup-backend)

;; https://elixirforum.com/t/emacs-elixir-setup-configuration-wiki/19196
;; (with-eval-after-load 'elixir-mode
;;   (spacemacs/declare-prefix-for-mode 'elixir-mode
;;     "mt" "tests" "testing related functionality")
;;   (spacemacs/set-leader-keys-for-major-mode 'elixir-mode
;;     "tb" 'exunit-verify-all
;;     "ta" 'exunit-verify
;;     "tk" 'exunit-rerun
;;     "tt" 'exunit-verify-single))

;; (with-eval-after-load 'elixir-ts-mode
;;   (spacemacs/declare-prefix-for-mode 'elixir-ts-mode
;;     "mt" "tests" "testing related functionality")
;;   (spacemacs/set-leader-keys-for-major-mode 'elixir-ts-mode
;;     "tb" 'exunit-verify-all
;;     "ta" 'exunit-verify
;;     "tk" 'exunit-rerun
;;     "tt" 'exunit-verify-single))

;; (push '("*exunit-compilation*"
;;         :dedicated t
;;         :position bottom
;;         :stick t
;;         :height 0.4
;;         :noselect t)
;;       popwin:special-display-config)

;; LSP 설정 관련 Configure emacs settings with lsp-mode
;; To configure through emacs (using lsp-mode), you can use code like the below.
;; (defvar lsp-elixir--config-options (make-hash-table))
;; (add-hook 'lsp-after-initialize-hook
;;           (lambda ()
;;             (lsp--set-configuration `(:elixirLS, lsp-elixir--config-options))))

;;;;; DONT javascript lsp disabled

;; (remove-hook 'js2-mode-local-vars-hook #'spacemacs//javascript-setup-backend)
;; (remove-hook 'js2-mode-local-vars-hook #'spacemacs//javascript-setup-next-error-fn)
;; (remove-hook 'js2-mode-local-vars-hook #'spacemacs//javascript-setup-dap)
;; (remove-hook 'js2-mode-hook #'spacemacs//javascript-setup-checkers)

```


#### <span class="section-num">4.1.4</span> Knowledge Graph : EKG LLM {#h:82947141-6546-4ef7-be01-3ca41bb02e87}

```elisp
;;;; TODO Knowledge Graph : EKG with LLM

;;;;; ekg use-package

(use-package triples)
(use-package llm)

(use-package ekg
  :ensure t
  :defer 2
  :config
  (setq ekg-db-file (concat org-directory "ekg/ekg.db"))
  (setq ekg-note-inline-max-words 200) ; default 500

  (require 'ekg-embedding)
  (ekg-embedding-generate-on-save)
  (require 'ekg-llm)

  (require 'llm-openai)  ;; The specific provider you are using must be loaded.
  ;; (require 'llm-gemini)

  (setq llm-warn-on-nonfree nil)

  (let ((my-provider (make-llm-openai :key my-openai-api-key)))
    (setq ekg-llm-provider my-provider
          ekg-embedding-provider my-provider))

  ;; (let ((my-provider (make-llm-gemini :key my-gemini-api-key)))
  ;;   (setq ekg-llm-provider my-provider
  ;;     ekg-embedding-provider my-provider))

  ;; (defun ash/capture-literature-note ()
  ;;   (interactive)
  ;;   (ekg-capture-url (ash/get-current-url) (ash/get-current-title)))

  ;; org-mode 를 고집할 필요가 있나?!
  ;; (setq ekg-capture-default-mode 'markdown-mode) ; default 'org-mode
  ;; (setq ekg-display-note-template
  ;;   "%n(id)%n(tagged)%n(titled)%n(text 500)%n(other)") ; default

  (unless *is-termux*
    ;; gleek-dotfiles-ekg/core/lang/core-org.el:802
    (defun +ekg-logseq-sync(&rest args)
      (require 'ekg-logseq)
      ;; (setq ekg-logseq-dir (concat +ekg-directory "logseq/"))
      (setq ekg-logseq-dir "~/sync/logseq/logseqfiles/")
      (ekg-logseq-sync))
    (add-hook 'ekg-note-save-hook '+ekg-logseq-sync))

  ;; ekg-features : tags
  ;; https://github.com/garyo/emacs-config/commit/1aacbcad7aaf47d2e7cb3fc2ff433bf864f6afc6
  (defun get-ekg-body-tags (note)
    "Get #tags from body of EKG note"
    (let* ((string (ekg-note-text note))
           (regexp "#\\([-_.a-zA-Z0-9]+\\)")
           matches
           (newtags (save-match-data
                      (let ((pos 0)
                            matches)
                        (while (string-match regexp string pos)
                          (push (match-string 1 string) matches)
                          (setq pos (match-end 0)))
                        matches))))
      (seq-uniq (append newtags (ekg-note-tags note)))))

  (defun my-ekg-note-pre-save-hook (note)
    "Apply #tags found in body to the note's tags"
    (let ((tags (get-ekg-body-tags note)))
      (message "Setting tags to %s" tags)
      ;; Workaround: the setf macro below doesn't work properly;
      ;; it macroexpands to a call to a function named
      ;; "(setf ekg-note-tags)" including the parens and spaces!
      ;; Just call aset to set the slot instead.
      ;; See https://emacs.stackexchange.com/questions/79007
      ;; (setf (ekg-note-tags note) tags)
      (aset note (cl-struct-slot-offset 'ekg-note 'tags) tags)
      (ekg--normalize-note note)
      ))

  ;; Allow a note to have tags in the body, by scanning the body before saving and adding any tags to the note's tags.
  (add-hook 'ekg-note-pre-save-hook 'my-ekg-note-pre-save-hook)

  ;; /ahyatt-dotfiles/.emacs.d/emacs.org:1098
  (defun ash/log-to-ekg (text &optional org-mode)
    "Log TEXT as a note to EKG's date, appending if possible."
    (let ((notes (ekg-get-notes-with-tags (list (ekg-tag-for-date) "log"))))
      (if notes
          (progn
            (message "ash/log-to-ekg...")
            (setf (ekg-note-text (car notes)) (concat (ekg-note-text (car notes)) "\n" text))
            (ekg-save-note (car notes)))
        (ekg-save-note (ekg-note-create :text text :mode (if org-mode 'org-mode 'text-mode)
                                        :tags `(,(ekg-tag-for-date) "log"))))))
  ) ;; end-of ekg

;;;;; ellama

(defvar emacs-llm-default-provider nil "The default LLM provider to use in Emacs.")
(use-package ellama
  :init
  (setopt ellama-language "Korean")
  (setopt ellama-provider emacs-llm-default-provider))

;;;;; hypothesis

(with-eval-after-load 'hypothesis
  (setq hypothesis-archive (concat denote-directory "20231218T151212--hypothesis__annotation_bookmark.org")))

```


#### <span class="section-num">4.1.5</span> Desktop TUI/GUI {#h:247cc222-eaed-4541-9f85-99f710d3b031}

```elisp
;;;; 'Desktop' - TUI/GUI

;; (my-define-custom-layout-per-machine) ; permachine.el

;; (persp-def-buffer-save/load
;;  :mode 'vterm-mode :tag-symbol 'def-vterm-buffer
;;  :save-vars '(major-mode default-directory))

(unless *is-termux*

;;;;; Load theme

  (my/switch-theme (car my/default-theme))

;;;;; Define custom-layout

  (progn
    (spacemacs|define-custom-layout "Emacs"
      :binding "e"
      :body

      (when (= 1 (length (tab-bar-tabs)))
        (tab-bar-new-tab)
        (tab-bar-new-tab)
        (tab-bar-new-tab)
        (tab-bar-new-tab)
        (tab-bar-rename-tab "agenda" 1)
        (tab-bar-rename-tab "denote" 2)
        (tab-bar-rename-tab "ekg" 3)
        (tab-bar-rename-tab "code" 4)
        (tab-bar-rename-tab "emacs" 5)
        (tab-bar-select-tab 2)
        ;; (dired denote-directory)
        (denote-sort-dired nil 'signature nil)
        (delete-other-windows)
        (split-window-right-and-focus)
        (org-roam-node-random)
        (tab-bar-select-tab 3)
        (ekg-show-notes-latest-modified)
        (delete-other-windows)
        (tab-bar-select-tab 4)
        (dired user-project-directory) ;; per-machine.el
        (tab-bar-select-tab 5)
        ;; (spacemacs/find-dotfile)
        (goto-emacs-dotfiles.org)
        (delete-other-windows)
        (tab-bar-select-tab 1)
        (org-agenda nil " ")
        ;; (junghan/youtube-setup-emacs-series)
        )
      )
    ) ; end of progn

  ;;   (spacemacs|define-custom-layout "@Translate"
  ;;     :binding "t"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       (tab-bar-select-tab 1)
  ;;       )

  ;;     ;; (find-file "~/sync/org/roam")
  ;;     ;; (find-file "~/sync/org/roam/_index.org") ; Blog Index
  ;;     ;; (split-window-right-and-focus)
  ;;     (spacemacs/window-split-single-column)
  ;;     (find-file "~/man")
  ;;     )

  ;;   (spacemacs|define-custom-layout "@Web"
  ;;     :binding "w"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       (tab-bar-select-tab 1)
  ;;       )

  ;;     ;; (find-file "~/sync/org/roam")
  ;;     ;; (find-file "~/sync/org/roam/_index.org") ; Blog Index
  ;;     ;; (split-window-right-and-focus)
  ;;     (spacemacs/window-split-single-column)
  ;;     (eww "junghanacs.com")
  ;;     )

  ;;   (spacemacs|define-custom-layout "@Code"
  ;;     :binding "c"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       (tab-bar-select-tab 1)
  ;;       )

  ;;     ;; (find-file "~/sync/org/roam")
  ;;     ;; (find-file "~/sync/org/roam/_index.org") ; Blog Index
  ;;     ;; (split-window-right-and-focus)
  ;;     (spacemacs/window-split-single-column)
  ;;     (find-file "~/code")
  ;;     )

  ;;   (spacemacs|define-custom-layout "@Bookmark"
  ;;     :binding "m"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       (tab-bar-select-tab 1)
  ;;       )
  ;;     ;; (list-bookmarks)
  ;;     (split-window-right-and-focus)
  ;;     (list-bookmarks)
  ;;     )

  ;;   (spacemacs|define-custom-layout "@Blog"
  ;;     :binding "b"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       (find-file "~/git/blog/_config.yml")
  ;;       (tab-bar-select-tab 1)
  ;;       )

  ;;     (spacemacs/window-split-single-column)
  ;;     (find-file "~/git/blog/_posts")
  ;;     )

  ;;   (spacemacs|define-custom-layout "@Feed"
  ;;     :binding "f"
  ;;     :body

  ;;     (when (= 1 (length (tab-bar-tabs)))
  ;;       (tab-bar-new-tab)
  ;;       ;; (pocket-reader)
  ;;       ;; (find-file "~/sync/org/elfeed/elfeed.org")
  ;;       (my/open-elfeed-list)
  ;;       (tab-bar-select-tab 1)
  ;;       )

  ;;     (elfeed)
  ;;     ;; (spacemacs/window-split-single-column)
  ;;     ;; (find-file "~/sync/org/elfeed/elfeed.org")
  ;;     )
;;;;; context-menu-mode on GUI

  (when (display-graphic-p) ;; gui
    (context-menu-mode 1)
    ;; (load-file (concat dotspacemacs-directory "site-lisp/url-bookmarks.el"))
    ;; (require 'cc-menu-loader)
    )

;;;;; local packages on Linux

  (when *is-linux*
    ;; (add-to-list 'load-path "~/sync/emacs/emacs-pkgs/unpackaged.el/")
    ;; (add-to-list 'load-path "~/emacs/forked-pkgs/hyperbole/")
    ;; (require 'hyperbole)

    ;; (add-to-list 'load-path "~/emacs/forked-pkgs/cnfonts/")
    ;; (require 'cnfonts)
    ;; cnfonts-편집-프로필을 사용하여 프로필을 수정합니다. 현재 사용 중인
    ;; 글꼴이 맞지 않는 경우 'cnfonts-edit-profile' 명령을 실행하여 현재
    ;; 프로필을 조정하면 이와 유사한 그래픽 인터페이스가 나타납니다:

    ;; ox-zenn ox-jegil ox-qmd
    ;; (add-to-list 'load-path "~/emacs/forked-pkgs/ox-zenn.el/")
    ;; (require 'ox-zenn)
    ;; (require 'ox-jegil)
    ;; (add-to-list 'load-path "~/emacs/forked-pkgs/ox-qmd/")
    ;; (require 'ox-qmd)

    ;; org-cv
    ;; M-x org-export-dispatch 함수를 호출하면 moderncv 메뉴가 보인다.
    ;; (use-package ox-moderncv
    ;;   :init (require 'ox-moderncv)
    ;;   )
    ;; zzamboni.org/vita/
    (add-to-list 'load-path "~/sync/emacs/emacs-pkgs/org-cv")

    (use-package ox-awesomecv
      :init (require 'ox-awesomecv))

    ) ; end-of is-linux and not is-termux

;;;;; DONT 1st Party Modes

;;;;;; Pair Programming

  ;; (defvar gopar-pair-programming nil)
  ;; (defun gopar/pair-programming ()
  ;;   "Poor mans minor mode for setting up things that i like to make pair programming easier."
  ;;   (interactive)
  ;;   (if gopar-pair-programming
  ;;       (progn
  ;;         ;; Don't use global line numbers mode since it will turn on in other modes that arent programming
  ;;         (dolist (buffer (buffer-list))
  ;;           (with-current-buffer buffer
  ;;             (when (derived-mode-p 'prog-mode)
  ;;               (display-line-numbers-mode -1))))
  ;;         (remove-hook 'prog-mode-hook 'display-line-numbers-mode)
  ;;         (neotree-hide)

  ;;         ;; disable all themes change to a friendlier theme
  ;;         (mapcar 'disable-theme custom-enabled-themes)
  ;;         (setq gopar-pair-programming nil))

  ;;     (progn
  ;;       ;; display line numbers
  ;;       (dolist (buffer (buffer-list))
  ;;         (with-current-buffer buffer
  ;;           (when (derived-mode-p 'prog-mode)
  ;;             (display-line-numbers-mode 1))))
  ;;       (add-hook 'prog-mode-hook 'display-line-numbers-mode)

  ;;       ;; disable all themes change to a friendlier theme
  ;;       (mapcar 'disable-theme custom-enabled-themes)
  ;;       (load-theme 'doom-shades-of-purple)
  ;;       (neotree-show)
  ;;       (setq gopar-pair-programming t))))


;;;;;; Streaming on Youtube

  (defun junghan/youtube-setup-emacs-series ()
    (interactive)
    (delete-other-windows)
    (my/switch-theme (car my/default-theme))
    (let ((dashboard-items '((agenda . 5) (fortune))))
      (dashboard-open))
    )

  ;; (defun gopar/youtube-setup-emacs-goodies-series ()
  ;;   (interactive)
  ;;   (delete-other-windows)
  ;;   (display-time-mode -1)
  ;;   (consult-theme 'doom-shades-of-purple)
  ;;   (let ((dashboard-items '((vocabulary) (recents . 5) (bookmarks . 5))))
  ;;     (dashboard-open)))

  ;; (defun gopar/youtube-setup-python-series ()
  ;;   (interactive)
  ;;   (delete-other-windows)
  ;;   (display-time-mode -1)
  ;;   (consult-theme 'doom-nord-aurora)
  ;;   (let ((dashboard-items '((vocabulary) (recents . 5) (bookmarks . 5))))
  ;;     (dashboard-open)))

  ) ; end-of unless termux

```


#### <span class="section-num">4.1.6</span> Android Termux {#h:fa6cf2a6-f0cf-463c-8b0c-09be24644cbd}

```elisp
;;;; 'Android' - Termux

(when *is-termux*

  (global-set-key (kbd "<M-SPC>") 'toggle-input-method)
  (global-set-key (kbd "M-<backtab>")
                  (lambda() (interactive) (other-window -1)))

  ;; simple format
  (setq org-agenda-prefix-format
        '((agenda  . " %i %?-12t% s")
          (todo  . " %i ")
          (tags  . " %i ")
          (search . " %i ")))

  (when (= 1 (length (tab-bar-tabs)))
    (tab-bar-new-tab)
    (tab-bar-new-tab)
    (tab-bar-rename-tab "Home" 1)
    (tab-bar-rename-tab "Agenda" 2)
    (tab-bar-rename-tab "Denote" 3)
    (my/switch-theme (car my/default-theme))
    (tab-bar-select-tab 2)
    (org-agenda nil " ")
    (delete-other-windows)
    (tab-bar-select-tab 3)
    ;; (dired denote-directory)
    (denote-sort-dired nil 'signature nil)
    (delete-other-windows)
    (tab-bar-select-tab 1)
    (spacemacs/home)
    )
  ) ;; when *is-termux*

;; android 일 때 폰트 설정하라.
;; (when *is-android*
;;   (message "Loading Android Emacs\n")
;;   ;; gpg: key 066DAFCB81E42C40: public key "GNU ELPA Signing Agent (2019) <elpasign@elpa.gnu.org>" imported

;;   ;; '(default ((t (:inherit nil :extend nil :stipple nil :background "#181a26" :foreground "gray80" :inverse-video nil :box nil :strike-through nil :overline nil :underline nil :slant normal :weight regular :height 119 :width normal :family "SEC Regular_SamsungKoreanR" :foundry "GOOG")))))
;;   (set-face-attribute 'default nil :family "SEC Regular_SamsungKoreanR" :foundry "GOOG" :width 'normal :weight 'regular :height 118)
;;   (set-fontset-font nil 'hangul (font-spec :family "SEC Regular_SamsungKoreanR" :foundry "GOOG"))
;;   (set-fontset-font t 'emoji (font-spec :family "Noto Color Emoji") nil)
;;   (set-fontset-font t 'emoji (font-spec :family "Noto Emoji") nil 'prepend) ; Top
;;   )

```


#### <span class="section-num">4.1.7</span> Else Temporary {#h:e879291d-f691-41fa-b2fe-b89e1ad467cb}

```elisp
;;;; musicbrainz

;; https://musicbrainz.org/account/applications
;; gtgkjh85@naver.com / gtgkjh
;; Application	Type	OAuth Client ID	OAuth Client Secret	Actions
;; emacs	Installed Application	2Lf0anF70FwJJmjtYL2ERFtRfLsXX161	tvqgUFa-GkSSjVdGEpTFIE3LVtZ_D6T1
```


#### <span class="section-num">4.1.8</span> end-of user-config {#h:df1e850f-3314-40cf-baa7-199d878b1e77}

```elisp
;;;; end-of user-config

;; Show 'Startup-Time'
;; (defun display-startup-echo-area-message ()
;;   "Display startup message."
;;   (message (concat "Startup time: " (emacs-init-time))))
```


### <span class="section-num">4.2</span> Keybindings (`user-keybindings.el`) {#h:00ee5f3d-d76b-4b74-bbb1-a6845ccac64a}




#### <span class="section-num">4.2.1</span> Custom HYDRA {#h:c70475af-92ce-4f0e-84b0-0f05298f478e}

```elisp
;;; -*- mode: emacs-lisp; lexical-binding: t -*-

;;;; Custom HYDRA

;;;;; Hydra All using pretty-hydra

;; define everything here
(require 'pretty-hydra)
;; (require 's)
(require 'all-the-icons)

;; with-faicon function allows an icon in hydra title. Requires following requires and aliases. To omit don't include 'with-faicon' in appearance-title
;; define an icon function with all-the-icons-faicon
;; to use filecon, etc, define same function with icon set
(defun with-faicon (icon str &rest height v-adjust)
  (s-concat (all-the-icons-faicon icon :v-adjust (or v-adjust 0) :height (or height 1)) " " str))
(defun with-fileicon (icon str &rest height v-adjust)
  (s-concat (all-the-icons-fileicon icon :v-adjust (or v-adjust 0) :height (or height 1)) " " str))

;;;;;; hydra-jumps

(pretty-hydra-define hydra-jumps
  (:color amaranth :exit t :quit-key "q")
  ("Jump visually"
   (("j" avy-goto-word-1 "to word" :exit t)
    ("l" avy-goto-line "to line" :exit t)
    ("c" avy-goto-char "to char" :exit t)
    ("r" avy-resume "resume" :exit t))
   "Jump via minibuffer"
   (("i" consult-imenu "imenu" :exit t)
    ("o" consult-outline "outline" :exit t))
   "Jump & go"
   (("u" ash/avy-goto-url "open url" :exit t))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;;;;;; hydra-structural : smartparens

  ;;;;; hydra-structural : puni
(pretty-hydra-define hydra-structural
  (:color amaranth :quit-key "q")
  ("Change"
   (("," puni-slurp-forward "slurp-forward")
    ("." puni-barf-forward "barf-forward")
    ("]" puni-slurp-forward "slurp-backward")
    ("[" puni-barf-forward "barf-backward")
    ("." puni-splice "splice")
    ("?" puni-convolute "convolute"))
   "Movement"
   (("a" puni-beginning-of-sexp "beginning of sexp")
    ("e" puni-end-of-sexp "end of sexp")
    (")" puni-syntactic-forward-punc "down sexp")
    ("(" puni-syntactic-backward-punc "up sexp"))
   "Formatting"
   (("z" puni-squeeze "squeeze/unwrap"))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;; (pretty-hydra-define hydra-structural
;;   (:color amaranth :quit-key "q")
;;   ("Change"
;;    (("i" sp-change-inner "change inner" :exit t)
;;     ("k" sp-kill-sexp "kill sexp")
;;     ("]" sp-slurp-hybrid-sexp "slurp")
;;     ("/" sp-swap-enclusing-sexp "swap enclusing"))
;;    "Movement"
;;    (("b" sp-beginning-of-sexp "beginning of sexp")
;;     ("e" sp-end-of-sexp "end of sexp")
;;     ("d" sp-down-sexp "down sexp")
;;     ("e" sp-up-sexp "up sexp"))
;;    "Formatting"
;;    (("r" sp-rewrap-sexp "rewrap"))
;;    "Misc"
;;    (("=" hydra-all/body "back" :exit t))))

;;;;;; hydra-multiple-cursors

(pretty-hydra-define hydra-multiple-cursors
  (:color amaranth :quit-key "q")
  ("Mark via region"
   (("l" mc/edit-lines "edit lines" :exit t)
    ("s" mc/mark-all-in-region-regexp "mark all in region re" :exit t))
   "Mark"
   (("a" mc/mark-all-like-this "mark all" :exit t)
    ("d" mc/mark-all-dwim "mark dwim" :exit t))
   "Mark incrementally"
   (("n" mc/mark-next-like-this "mark next like this")
    ("N" mc/skip-to-next-like-this "skip to next like this")
    ("M-n" mc/unmark-next-like-this "unmark next like this")
    ("p" mc/mark-previous-like-this "mark previous like this")
    ("P" mc/skip-to-previous-like-this "skip to previous like this")
    ("M-p" mc/unmark-previous-like-this "unmark previous like this")
    ("L" mc/mark-next-lines "mark next lines"))
   "Insert"
   (("0" mc/insert-numbers "insert numbers" :exit t)
    ("A" mc/insert-letters "insert letters" :exit t))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;;;;;; hydra-expand

(pretty-hydra-define hydra-expand
  (:color amaranth :quit-key "q")
  ("Expand/Contract"
   (("e" er/expand-region "expand")
    ("c" er/contract-region "contract"))
   "Expand to..."
   (("d" er/mark-defun "defun")
    ("\"" er/mark-inside-quotes "quotes")
    ("'" er/mark-inside-quotes "quotes")
    ("p" er/mark-inside-pairs "pairs")
    ("." er/mark-method-call "call"))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;;;;;; DONT hydra-denote

(pretty-hydra-define hydra-denote
  (:color amaranth :exit t :quit-key "q"
          :pre (progn (setq which-key-inhibit t))
          :post (progn (setq which-key-inhibit nil)))
  ("new"
   (("n" denote-create-note-using-signature "create-note-using-signature")
    ("t" denote-create-note-with-template "create-note-with-template")
    ("d" denote-create-note-using-date "create-note-using-date"))
   "link"
   (("i" denote-link "insert link")                  ;mnemonic "insert"
    ("c" denote-link-after-creating "link to new"))  ;mnemonic "create"
   "inspect & open"
   (("l" denote-link-find-file "open linked file")   ;mnemonic "link"
    ("b" denote-link-find-backlink "open backlink")  ;mnemonic "backlink"
    ("s" consult-notes "search zettels")
                                        ;("f" (consult-ripgrep denote-directory) "full text")
    ("f" consult-notes-search-in-all-notes "full text"))
   "modify"
   (("r" denote-rename-file "rename")
    ("u" denote-rename-file-using-front-matter "rename using front-matter"))
   ))

;;;;;; hydra-ekg

(pretty-hydra-define hydra-ekg ()
  ("Navigation"
   (("t" ekg-show-notes-for-today "show today" :exit t)
    ("g" ekg-show-notes-with-tag "show tag" :exit t)
    ("r" ekg-show-notes-latest-captured "show latest" :exit t)
    ("b" ekg-embedding-show-similar-to-current-buffer "show similar to current buffer" :exit t)
    ("s" ekg-embedding-search "embedding-search" :exit t)
    )
   "Capture"
   (("k" ekg-capture)
    ("u" ekg-capture-url)
    ("f" ekg-capture-file)
    ;; ("u" ash/capture-literature-note)
    )
   ))

;;;;;; hydra-yas

(pretty-hydra-define hydra-yas ()
  ("Snippets"
   (("n" yas-new-snippet "new" :exit t)
    ("r" yas-reload-all "reload" :exit t)
    ("v" yas-visit-snippet-file "visit" :exit t))
   "Movement"
   (("f" yas-next-field "forward field" :exit nil)
    ("b" yas-prev-field "previous field" :exit nil))))

;;;;;; hydra-flycheck
(pretty-hydra-define hydra-flycheck ()
  ("Movement"
   (("n" flymake-goto-next-error "next error")
    ("p" flymake-goto-prev-error "previous error")
    ("d" flymake-goto-diagnostic "diagnostic")
    ("<" flycheck-previous-error "previous flycheck error")
    (">" flycheck-next-error "next flycheck error")
    ("l" flycheck-list-errors "list")
    ("." consult-flymake))
   "Display"
   (("." flymake-show-diagnostic "show diagnostic")
    ("B" flymake-show-diagnostics-buffer "diagnostics buffers"))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;;;;;; hydra-mail

;; notmuch is too specialized to be set up here, it varies from machine to
;; machine. At some point I should break it down into the general &
;; specialized parts.
(defun ash/inbox ()
  (interactive)
  (notmuch-search "tag:inbox" t))
(pretty-hydra-define hydra-mail ()
  ("Search"
   (("s" notmuch-search "search" :exit t)
    ("h" consult-notmuch "incremental search" :exit t))
   "Application"
   (("n" notmuch-hello "notmuch" :exit t)
    ("i" ash/inbox "inbox" :exit t)
    ("c" notmuch-mua-new-mail "compose" :exit t))
   "Misc"
   (("=" hydra-all/body "back" :exit t))))

;;;;;; hydra-org-main

(pretty-hydra-define hydra-org-main ()
  ("Misc"
   (("a" org-agenda "agenda" :exit t)
    ("c" org-capture "capture" :exit t))
   "Links"
   (("s" org-store-link "store" :exit t))))

;;;;;; hydra-find

(pretty-hydra-define hydra-find ()
  ("In-Buffer"
   (("i" consult-imenu "imenu" :exit t)
    ("m" consult-mark "mark rings" :exit t)
    ("o" consult-multi-occur "occur" :exit t)
    ("e" consult-flycheck "errors" :exit t)
    ("l" consult-goto-line "line" :exit t))
   "Other"
   (("r" consult-ripgrep "grep" :exit t)
    ("b" consult-bookmark "bookmark" :exit t)
    ("R" consult-register "register" :exit t)
    ("C" consult-complex-command "complex command" :exit t))))

;;;;;; hydra-toggles

(defvar hydra-toggles--title (with-faicon "toggle-on" "Toggles"))
(pretty-hydra-define hydra-toggles
  (:color amaranth :quit-key "<espace>" :title hydra-toggles--title)
  ;; (pretty-hydra-define hydra-toggles ()
  ("Basic"
   (("n" linum-mode "line number" :toggle t)
    ("w" whitespace-mode "whitespace" :toggle t)
    ("W" whitespace-cleanup-mode "whitespace cleanup" :toggle t)
    ("r" rainbow-mode "rainbow" :toggle t)
    ("L" page-break-lines-mode "page break lines" :toggle t))
   "Highlight"
   (("s" symbol-overlay-mode "symbol" :toggle t)
    ("l" hl-line-mode "line" :toggle t)
    ("x" highlight-sexp-mode "sexp" :toggle t)
    ("t" hl-todo-mode "todo" :toggle t))
   "Coding"
   (
    ;; ("p" smartparens-mode "smartparens" :toggle t)
    ;; ("P" smartparens-strict-mode "smartparens strict" :toggle t)
    ;; ("S" show-smartparens-mode "show smartparens" :toggle t)
    ("f" flycheck-mode "flycheck" :toggle t))
   "Emacs"
   (("D" toggle-debug-on-error "debug on error" :toggle (default-value 'debug-on-error))
    ("X" toggle-debug-on-quit "debug on quit" :toggle (default-value 'debug-on-quit)))))

;;;;;; hydra-all

(pretty-hydra-define hydra-all
  (:quit-key "<escape>" :title "All")
  ("Applications"
   (("m" hydra-mail/body "mail" :exit t)
    ("o" hydra-org-main/body "org" :exit t)
    ;; ("d" hydra-denote/body "denote" :exit t)
    ;; ("r" hydra-roam/body "roam" :exit t)
    ("k" hydra-ekg/body "ekg" :exit t)
    ;; ("S" hydra-straight/body "straight" :exit t)
    ;; ("!" ash/el-secretario-daily-review "secretary" :exit t)
    ("g" magit-status "magit" :exit t))
   "Editing"
   (("c" hydra-multiple-cursors/body "multiple cursors" :exit t)
    ("s" hydra-structural/body  "structural" :exit t)
    ("p" hydra-expand/body "expand region" :exit t)
    ("y" hydra-yas/body "snippets" :exit t))
   "Movement"
   (("j" hydra-jumps/body "jumps" :exit t)
    ("E" hydra-flycheck/body "errors" :exit t)
    ("G" deadgrep "grep" :exit t))
   "Misc"
   (("t" hydra-toggles/body "toggles" :exit t)
    ("q" nil "Quit" :color red :exit t)
    ("f" hydra-find/body "find" :exit t))
   ))

;;;;; hydra-jump-to-directory

(defhydra hydra-jump-to-directory
  (:color amaranth :exit t :quit-key "<escape>")
  "Jump to directory"

  ("b" (find-file "~/git/blog") "blog")
  ("n" (find-file "~/git/notes") "notes")
  ("c" (find-file "~/nosync/clone-notes/") "clone-notes")
  ("C" (find-file "~/sync/markdown/cheat") "cheat")
  ("m" (find-file "~/sync/man") "man")
  ("o" (find-file "~/sync/org/") "org")
  ("r" (find-file "~/sync/org/roam") "org-roam")
  ("s" (find-file "~/.spacemacs.d/snippets/") "snippets")

  ("v" (find-file "~/Videos") "Videos")
  ("p" (find-file "~/Pictures") "Pictures")
  ("d" (find-file "~/Documents") "Documents")
  ("D" (find-file "~/Downloads") "Downloads")
  ("P" (find-file "~/Public") "Public")
  ("t" (find-file "~/Templates") "Templates")
  ("q" nil "Quit" :color blue))

;;;;; hydra-jump-to-system-file

(defhydra hydra-jump-to-system-file
  (:color amaranth :exit t :quit-key "<escape>")
  "Jump to system file"

  ("a" (find-file org-refile-file) "Inbox")
  ("n" (find-file org-now-file) "Now")
  ("l" (find-file org-links-file) "Links")
  ("c" (org-contacts-find-file) "Contacts")
  ("t" (find-file org-tags-file) "Tags")
  ("p" (find-file org-projectile-file) "Project")
  ("b" (find-file "~/.bashrc") "bashrc")
  ("z" (find-file "~/.zshrc") "zshrc")
  ("u" (find-file "~/sync/org/elfeed/elfeed.org") "elfeed.org")
  ("q" nil "Quit" :color blue))
```


#### <span class="section-num">4.2.2</span> Transient {#h:4e3f9ce0-12ea-4f22-ac7f-eb1816e6dec1}

```elisp

;;;; Transient

(require 'transient)

;;;;; EKG : setup-ekg-transients

;; https://github.com/ahyatt/ekg/discussions/100
(defun setup-ekg-transients () "Set up Transient menus for EKG"
       (transient-define-prefix my/ekg-dispatch ()
         "Top level Transient menu for EKG (Emacs Knowledge Graph)"
         [["Show"
           ("st" "Today" ekg-show-notes-for-today)
           ("slc" "Latest Captured" ekg-show-notes-latest-captured)
           ("slm" "Latest Mod" ekg-show-notes-latest-modified)
           ("sx" "Trash" ekg-show-notes-in-trash)
           ("sd" "Drafts" ekg-show-notes-in-drafts)
           "Find Tags"
           ("tt" "Tag" ekg-show-notes-with-tag)
           ("taa" "All Tags" ekg-show-notes-with-all-tags)
           ("ta?" "Any Tag" ekg-show-notes-with-any-tags)
           ]
          ["Capture"
           ("cc" "New Note" ekg-capture)
           ("cu" "...from URL" ekg-capture-url)
           ("cb" "...from current buffer" ekg-capture-file)
           ]
          ["Query" :if (lambda () (or (featurep 'ekg-llm) (featurep 'ekg-embedding)))
           ("qt" "for terms" ekg-embedding-search :if (lambda () (featurep 'ekg-embedding)))
           ("qb" "similar to current buffer" ekg-embedding-show-similar-to-current-buffer :if (lambda () (featurep 'ekg-embedding)))
           ("qR" "Regenerate embeddings" ekg-embedding-generate-all :if (lambda () (featurep 'ekg-embedding)))
           "AI"
           ("aq" "AI query, all notes" ekg-llm-query-with-notes :if (lambda () (featurep 'ekg-llm)))
           ]
          ["Misc"
           ("gr" "Global rename tag" ekg-global-rename-tag)
           ("e" "This note ..." my/ekg-notes-dispatch :if-mode ekg-notes-mode)
           ("Q" "Quit this menu" transient-quit-one)
           ]
          ])

       (transient-define-prefix my/ekg-notes-dispatch ()
         "Notes buffer Transient menu for EKG (Emacs Knowledge Graph)"
         [["Show Notes"
           ("sa" "with any of this note's tags" ekg-notes-any-note-tags)
           ("sA" "with any of these notes' tags" ekg-notes-any-tags)
           ("st" "select tag" ekg-notes-tag)
           ("ss" "search for similar" ekg-embedding-show-similar :if (lambda () (featurep 'ekg-embedding)))
           ]
          ["AI"
           ("aa" "AI send & append" ekg-llm-send-and-append-note :if (lambda () (featurep 'ekg-llm)))
           ("ar" "AI send & replace" ekg-llm-send-and-replace-note :if (lambda () (featurep 'ekg-llm)))
           ]
          ["Manage"
           ("c" "create" ekg-notes-create)
           ("d" "delete" ekg-notes-delete)
           ("g" "refresh" ekg-notes-refresh)
           ("k" "kill (hide) note" ekg-notes-kill)
           ("o" "open/edit" ekg-notes-open)
           ("m" "Change mode of current note" ekg-change-mode)
           ]
          ["Browse"
           ("b" "browse resource" ekg-notes-browse)
           ("u" "Browse to URL" ekg-browse-url)
           ("B" "select & browse" ekg-notes-select-and-browse-url)
           ]
          ["Global"
           ("g" "global ekg commands..." my/ekg-dispatch)
           ("q" "quit this menu" transient-quit-one)
           ("Q" "quit EKG" kill-buffer-and-window)
           ]
          ])
       )

```


#### <span class="section-num">4.2.3</span> Keybindings {#h:de063378-0e06-4f78-ae73-4c30787fc112}

```elisp
;;;; Keybindings

;;;;; Basics

(global-set-key (kbd "<f1>") 'hydra-all/body)
;; (global-set-key (kbd "<f1>") 'my/ekg-dispatch)

(global-set-key (kbd "<f2>") 'eval-last-sexp)
;; spacemacs/eval-current-form-to-comment-sp

;; Very convenient to close current buffer
;; 확인하자. 메이저 모드 키에 충돌 날라.
;; (define-key evil-normal-state-map (kbd ",`") #'spacemacs/kill-this-buffer)

(global-set-key (kbd "C-M-i") 'completion-at-point)
(global-set-key (kbd "C-M-:") 'pp-eval-expression)

;; false input
;; (global-unset-key (kbd "M-ESC ESC")) ; 'keyboard-escape-quit
(global-set-key (kbd "M-ESC ESC") 'keyboard-quit)

;; turn off kill emacs binding
;; (global-unset-key (kbd "C-x C-c")) ; save-buffer-and-kill-emacs

;; Kill this buffer now!
(global-set-key (kbd "M-S-q") 'spacemacs/kill-this-buffer)
;; 실수로 누르게 되는 빠른 종료 바인딩을 제거한다.
(spacemacs/set-leader-keys "qq" nil) ; prompt-kill-emacs

(global-set-key (kbd "C-`") #'spacemacs/default-pop-shell) ; vscode

;;;;; P +Pandoc/Export

;; export this file on buffer
(spacemacs/set-leader-keys "Pe" #'org-export-dispatch)
(spacemacs/set-leader-keys "Pr" #'org-reveal-export-to-html)
(spacemacs/set-leader-keys "Pp" #'org-hugo-export-wim-to-md)

;;;;; Super + Keybindings

;; 편집 창 포커스 이동을 간단하게
(progn
  (global-set-key (kbd "M-s-l") 'evil-window-right)
  (global-set-key (kbd "M-s-h") 'evil-window-left)
  (global-set-key (kbd "M-s-k") 'evil-window-up)
  (global-set-key (kbd "M-s-j") 'evil-window-down))

;; 2023-11-10 잘 안쓰게 되네? 고민해보자.
;; Tab-bar with C-<tab> + more key bindings
(progn
  (global-set-key (kbd "C-s-h") 'tab-previous)
  (global-set-key (kbd "C-<backtab>") 'tab-previous)
  (global-set-key (kbd "C-s-l") 'tab-next)
  (global-set-key (kbd "s-\\") 'tab-bar-switch-to-tab)
  ;; (global-set-key (kbd "C-s-1") #'(lambda() (interactive) (tab-bar-select-tab 1)))
  ;; (global-set-key (kbd "C-s-2") #'(lambda() (interactive) (tab-bar-select-tab 2)))
  ;; (global-set-key (kbd "C-s-3") #'(lambda() (interactive) (tab-bar-select-tab 3)))
  ;; (global-set-key (kbd "C-s-4") #'(lambda() (interactive) (tab-bar-select-tab 4)))
  ;; (global-set-key (kbd "C-s-5") #'(lambda() (interactive) (tab-bar-select-tab 5)))
  ;; (global-set-key (kbd "C-s-6") #'(lambda() (interactive) (tab-bar-select-tab 6)))
  ;; (global-set-key (kbd "C-s-7") #'(lambda() (interactive) (tab-bar-select-tab 7)))
  )

;; If you use a window manager be careful of possible key binding clashes
(global-set-key (kbd "M-<tab>") 'other-window) ; very useful
(global-set-key (kbd "M-<iso-lefttab>") (lambda() (interactive) (other-window -1))) ; == M-S-<tab>
(global-set-key (kbd "M-<backtab>") (lambda() (interactive) (other-window -1))) ; for terminal

(with-eval-after-load 'magit-section
  ;; (setq-default magit-section-highlighted-sections t)
  ;; (setq-default magit-section-highlight-overlays t)
  (define-key magit-section-mode-map (kbd "M-<tab>") 'other-window)
  (define-key magit-section-mode-map (kbd "C-<tab>") 'tab-next)
  )

;; TODO Check!
;; (global-set-key (kbd "s-`'") #')
;; (global-set-key (kbd "s-9") 'spacemacs/switch-to-minibuffer-window)

;; (global-set-key (kbd "C-1") 'kill-this-buffer)
;; (global-set-key (kbd "C-<down>") (kbd "C-u 1 C-v"))
;; (global-set-key (kbd "C-<up>") (kbd "C-u 1 M-v"))
;; (global-set-key (kbd "M-/") #'hippie-expand)
;; (global-set-key (kbd "C-x C-j") 'dired-jump) ; what is?
;; (global-set-key (kbd "C-c r") 'remember) ; what is?

;; Confession time: vi's killing up to a char is better than emacs, so let's change things.
(global-set-key (kbd "M-z") #'zap-up-to-char)

(progn
  (global-set-key (kbd "C-c j a") 'edit-abbrevs)
  ;; (global-set-key (kbd "C-c j c") 'my/open-literate-org)
  (global-set-key (kbd "C-c j c") 'org-contacts-find-file)
  (global-set-key (kbd "C-c j d") 'my/open-hunspell-personal)
  (global-set-key (kbd "C-c j D") 'my/open-dict-ko-mydata)
  (global-set-key (kbd "C-c j e") 'my/open-external)
  (global-set-key (kbd "C-c j E") 'my/open-elfeed-list)
  (global-set-key (kbd "C-c j r") 'my/open-refile)
  (global-set-key (kbd "C-c j q") 'my/open-quote)
  (global-set-key (kbd "C-c j m") 'my/open-mobile)
  (global-set-key (kbd "C-c j i") 'my/open-iam)
  (global-set-key (kbd "C-c j n") 'my/open-now)
  (global-set-key (kbd "C-c j l") 'my/open-links)
  (global-set-key (kbd "C-c j t") 'my/open-tags)
  (global-set-key (kbd "C-c j T") 'my/open-tempel-templates)
  (global-set-key (kbd "C-c j R") 'my/open-dotsamples-readme)
  (global-set-key (kbd "C-c j p") 'my/open-org-project)
  (global-set-key (kbd "C-c j Q") 'my/open-fortune-quotes)
  (global-set-key (kbd "C-c j w") 'my/open-org-workflow-path))

;; (global-set-key (kbd "C-c j C") 'my/open-csaroid-list)
;; (global-set-key (kbd "C-c j i") 'my/open-IAM-org)
;; (global-set-key (kbd "C-c j w") 'my/open-dotworkflow-org)
;; (global-set-key (kbd "C-c j h") 'my/browse-hugo-maybe)

;;;;; spacemacs declare-prefix
;; 여기에 관리해야 하는 파일 더 넣어야 할 지도 glossary tags url people

(spacemacs/declare-prefix
  "!"   "shell cmd"
  ;; "TAB" "Last buf"
  "\""  "terminal here"
  "#"   "register"
  "'"   "open shell"
  "*"   "search w/"
  "/"   "search w/o"
  "0"   "treemacs"
  ";"   "latest-popup"
  "²"   "select-window"
  "`"   "select-window"
  ":"   "kill-last-popup"
  "?"   "show bindings"
  "a"   "applications"
  "b"   "buffers"
  "c"   "codes"
  "C"   "Capture/Colors"
  "d"   "debug"
  "D"   "Diff/Compare"
  "e"   "errors"
  ;; "E"   "Export"
  "f"   "files"
  "F"   "Frames"
  "g"   "git/vc"
  "h"   "help"
  "i"   "insertion"
  "j"   "jump/join"
  "k"   "lisp"
  "K"   "Macros"
  "l"   "layouts"
  "m"   "major-mode"
  "n"   "narrow/numbers"
  "N"   "Navigation"
  "o"   "user bindings"
  "p"   "projects"
  "P"   "Pandoc/export"
  "q"   "quit"
  "r"   "regs/rings"
  "s"   "search/symbol"
  "S"   "Spelling"
  "t"   "toggles<1>"
  "T"   "Themes/UI"
  "C-t" "toggles<2>"
  "C-v" "rectangles"
  "u"   "universals"
  "v"   "er/expand"
  "w"   "windows"
  "x"   "text"
  "z"   "zoom"

  "<up>"   "window↑"
  "<down>"   "window↓"
  "<left>"   "window←"
  "<right>"   "window→"
  )

;;;;; M-g bindings (goto-map)

;; (global-set-key (kbd "M-g 1") 'hydra-all/body)
;; (global-set-key (kbd "M-g 2") 'major-mode-hydra)

(global-set-key (kbd "M-g 1") 'hydra-jump-to-directory/body)
(global-set-key (kbd "M-g 2") 'hydra-jump-to-system-file/body)
;; (global-set-key (kbd "M-g 3") 'my-org-hydra/body)
(global-set-key (kbd "M-g F") 'consult-flycheck)

;; Bookmark with BM layer
(global-set-key (kbd "M-g b") 'spacemacs/bm-transient-state/body)
(spacemacs/set-leader-keys "f." 'spacemacs/bm-transient-state/body)

;;;;; M-s bindings (search-map)

;; TODO

;;;;; emacs-lisp-mode

(spacemacs/set-leader-keys-for-major-mode 'emacs-lisp-mode
  "te" 'ert/eval-and-run-all-tests-in-buffer)

;;;;; org-mode


;; Org-mode (citar and org-roam)
(spacemacs/declare-prefix-for-mode 'org-mode "mB" "Bib/citar")
;; (spacemacs/declare-prefix-for-mode 'org-mode "mP" "Paragraph")
;; (spacemacs/set-leader-keys-for-major-mode 'org-mode
;;   "Ba" 'citar-org-roam-ref-add

(spacemacs/set-leader-keys-for-major-mode 'org-mode
  "/" 'math-preview-at-point
  "M-/" 'math-preview-all
  "C-M-/" 'math-preview-clear-all
  "iT" 'bh/insert-inactive-timestamp
  "ic" 'citar-insert-citation
  "er" 'org-reveal-export-to-html
  ;; "ep" 'org-hugo-export-wim-to-md
  "Bi" 'citar-insert-citation
  "Br" 'citar-insert-reference
  "Bn" 'citar-create-note
  "Bo" 'citar-open-note
  "mt" 'org-tidy-mode
  "mB" 'palimpsest-move-region-to-bottom
  "mT" 'palimpsest-move-region-to-top
  "mD" 'palimpsest-move-region-to-trash
  "mc" 'org-columns
  "mC" 'org-columns-quit
  ;; "mN" 'org-next-visible-heading
  ;; "mP" 'org-previous-visible-heading
  )

;; (spacemacs/set-leader-keys-for-major-mode 'emacs-lisp-mode
;;   "mN" 'outline-next-visible-heading
;;   "mP" 'outline-previous-visible-heading
;;   )

;;;;; eww-mode

(spacemacs/set-leader-keys-for-major-mode 'eww-mode
  "s" 'ace-link-eww
  "c" 'eww-copy-page-url
  )

(define-key help-mode-map "o" 'ace-link-help)

;; preview info files
(add-to-list 'auto-mode-alist '("\\.info\\'" . Info-on-current-buffer))
(evil-define-key 'normal Info-mode-map "o" 'ace-link-info)

;; 혹시 아래서도 안되면 위와 같이 잡아주라
;; (with-eval-after-load 'woman
;;   (define-key woman-mode-map "o" 'link-hint-open-link))
;; (with-eval-after-load 'eww
;;   (define-key eww-link-keymap "o" 'ace-link-eww)
;;   (define-key eww-mode-map "o" 'ace-link-eww)))))

;;;;; Tab for Heading : outline-mode and org-mode
;; search to narrow with heading and tag base on built-in outline-mode

(when (locate-library "outli")
  (with-eval-after-load 'outli
    ;; evil normal keybinding is perfer
    ;; (evil-define-key '(normal visual) outli-mode-map (kbd "S-<tab>") `(menu-item "" ,(lambda () (interactive) (outline-cycle -1)) :filter outli--on-heading))
    ;; (evil-define-key '(normal visual) outli-mode-map (kbd "S-TAB") `(menu-item "" ,(lambda () (interactive) (outline-cycle -1)) :filter outli--on-heading))
    ;; (evil-define-key '(normal visual) outli-mode-map (kbd "<backtab>") `(menu-item "" ,(lambda () (interactive) (outline-cycle -1)) :filter outli--on-heading))
    ;; (evil-define-key '(normal visual) outli-mode-map (kbd "S-<iso-lefttab>") `(menu-item "" ,(lambda () (interactive) (outline-cycle -1)) :filter outli--on-heading))

    (evil-define-key '(normal visual) outli-mode-map (kbd "S-<tab>") 'recenter-top-bottom)
    (evil-define-key '(normal visual) outli-mode-map (kbd "S-TAB") 'recenter-top-bottom)
    (evil-define-key '(normal visual) outli-mode-map (kbd "<backtab>") 'recenter-top-bottom)
    (evil-define-key '(normal visual) outli-mode-map (kbd "S-<iso-lefttab>") 'recenter-top-bottom)

    ;; 'TAB' for terminal emacs
    (evil-define-key '(normal visual) outli-mode-map (kbd "<tab>") `(menu-item "" outline-cycle :filter outli--on-heading))
    (evil-define-key '(normal visual) outli-mode-map (kbd "TAB") `(menu-item "" outline-cycle :filter outli--on-heading))

    (evil-define-key '(normal visual) prog-mode-map (kbd "<tab>") 'indent-for-tab-command)
    (evil-define-key '(normal visual) prog-mode-map (kbd "TAB") 'indent-for-tab-command)

    (evil-define-key '(normal) outli-mode-map (kbd "C-c 1") (lambda () (interactive) (outline--show-headings-up-to-level 1)))
    (evil-define-key '(normal) outli-mode-map (kbd "C-c 2") (lambda () (interactive) (outline--show-headings-up-to-level 2)))
    (evil-define-key '(normal) outli-mode-map (kbd "C-c 3") (lambda () (interactive) (outline--show-headings-up-to-level 3)))
    (evil-define-key '(normal) outli-mode-map (kbd "C-c 4") (lambda () (interactive) (outline--show-headings-up-to-level 4)))
    (evil-define-key '(normal) outli-mode-map (kbd "C-c 5") (lambda () (interactive) (outline--show-headings-up-to-level 5)))

    (evil-define-key '(normal) outli-mode-map (kbd "C-M-<tab>") 'outline-cycle-buffer)

    ;; (define-key outli-mode-map (kbd "C-M-<iso-lefttab>")
    ;;             (lambda () (interactive) (outline-cycle-buffer)))

    (evil-define-key '(normal insert) outli-mode-map (kbd "C-n") 'outline-next-visible-heading) ; this works
    (evil-define-key '(normal insert) outli-mode-map (kbd "C-p") 'outline-previous-visible-heading)

    (define-key prog-mode-map (kbd "C-c H") 'outline-insert-heading)
    (define-key prog-mode-map (kbd "C-c o") 'consult-outline)
    )
  )

;; org-mode
(with-eval-after-load 'org
  (define-key org-mode-map (kbd "<f3>") 'org-toggle-link-display)
  (define-key org-mode-map (kbd "<f4>") 'org-toggle-inline-images)

  (define-key org-mode-map (kbd "S-<tab>") (lambda () (interactive) (org-cycle 'FOLDED)))
  (define-key org-mode-map (kbd "S-TAB") (lambda () (interactive) (org-cycle 'FOLDED)))
  (define-key org-mode-map (kbd "<backtab>") (lambda () (interactive) (org-cycle 'FOLDED)))
  (define-key org-mode-map (kbd "S-<iso-lefttab>") (lambda () (interactive) (org-cycle 'FOLDED)))
  (define-key org-mode-map (kbd "C-M-<tab>") 'org-shifttab)

  (define-key org-mode-map (kbd "C-c 1") 'org-show-level-1)
  (define-key org-mode-map (kbd "C-c 2") 'org-show-level-2)
  (define-key org-mode-map (kbd "C-c 3") 'org-show-level-3)
  (define-key org-mode-map (kbd "C-c 4") 'org-show-level-4)
  (define-key org-mode-map (kbd "C-c 5") 'org-show-level-5)

  (define-key org-mode-map (kbd "C-c H") 'org-insert-heading)
  (define-key org-mode-map (kbd "C-c S") 'org-insert-subheading)

  ;; (evil-define-key '(normal insert visual) org-mode-map (kbd "C-c S") 'org-insert-subheading)
  ;; (evil-define-key '(normal insert visual) org-mode-map (kbd "RET") 'scimax/org-return)
  )

;;;;; 'SPC i' insertion

(spacemacs/set-leader-keys "it" 'hl-todo-insert)

;;;;; 'SPC a' applications

;; Hammy
(when (locate-library "hammy")
  (spacemacs/declare-prefix "ah"  "hammy-timer")
  (spacemacs/set-leader-keys
    "ahs" 'hammy-start
    "ahS" 'hammy-stop
    "ahn" 'hammy-next
    "ahl" 'hammy-view-log
    "ahr" 'hammy-reset
    "ahm" 'hammy-mode))

;; TMR Timer
(when (locate-library "tmr")
  (spacemacs/set-leader-keys "att" nil)
  (spacemacs/declare-prefix "att" "tmr")
  (spacemacs/set-leader-keys
    "attt" #'tmr
    "attl" #'tmr-tabulated-view
    "attc" #'tmr-clone
    "attk" #'tmr-cancel
    "atts" #'tmr-reschedule
    "atte" #'tmr-edit-description
    "attr" #'tmr-remove
    "attR" #'tmr-remove-finished
    "attT" #'tmr-with-description
    ))

;;;;; 'SPC g' git/vc

(when (locate-library "git-cliff")
  (spacemacs/set-leader-keys "g/" 'git-cliff-menu))

(when (locate-library "blamer")
  ;; (spacemacs/set-leader-keys "g9" 'blamer-show-commit-info)
  (spacemacs/set-leader-keys "g0" 'blamer-mode)
  (spacemacs/set-leader-keys "gb" 'blamer-mode)
  (spacemacs/set-leader-keys "gB" 'spacemacs/git-blame-transient-state/body)
  )

;;;;; 'SPC y'

;; Hypothesis
(when (locate-library "hypothesis")
  (spacemacs/declare-prefix "yh"  "hypothesis")
  (spacemacs/set-leader-keys
    "yho" #'hypothesis-to-org
    "yha" #'hypothesis-to-archive
    )
  )

;;;;; 'SPC f'

(spacemacs/set-leader-keys "fed" 'goto-emacs-dotfiles.org)

;;;;; unless is-termux

(unless *is-termux*
  (define-key projectile-mode-map (kbd "C-x p") 'projectile-command-map))
;; (setq projectile-project-search-path '("~/workspace/" "~/workspace/github" ("~/Exercism/emacs-lisp/" . 1)))
;;;;; custom and more

(spacemacs/set-leader-keys "o9" 'sort-lines)
;; (spacemacs/set-leader-keys "ow" 'eww)

;; Revert buffer - loads in .dir-locals.el changes
(spacemacs/set-leader-keys "oR" 'revert-buffer)
(spacemacs/set-leader-keys "of" 'fortune-message)

(spacemacs/set-leader-keys "oo" 'pulsar-highlight-line)
;; (spacemacs/set-leader-keys "oo" 'outline-minor-mode)

(spacemacs/set-leader-keys "ov" 'spacemacs/shell-pop-vterm)

(spacemacs/set-leader-keys
  "pm"      'magit-project-status
  )

;; C-x C-0 restores the default font size
(spacemacs/set-leader-keys "tS" #'text-scale-mode)

(spacemacs/declare-prefix "aoB"  "Bib/citar")
(spacemacs/set-leader-keys
  "aoBn" 'citar-create-note
  "aoBo" 'citar-open-note)

(spacemacs/declare-prefix "ia"  "abbrev-mode")
(spacemacs/set-leader-keys
  "iag" 'add-global-abbrev
  "ia'" 'expand-abbrev
  "iac" 'company-abbrev
  "ial" 'list-abbrevs
  "ia+" 'add-mode-abbrev)

(define-key dired-mode-map (kbd "<return>") 'dired-find-file-other-window)

;;;;; vterm

(when (locate-library "vterm")
  (with-eval-after-load 'vterm
    (define-key vterm-mode-map (kbd "<f1>") nil)
    (define-key vterm-mode-map (kbd "<f2>") nil)
    (define-key vterm-mode-map (kbd "<f3>") nil)
    (define-key vterm-mode-map (kbd "<f4>") nil)
    (define-key vterm-mode-map (kbd "<f5>") nil)
    (define-key vterm-mode-map (kbd "<f6>") nil)
    (define-key vterm-mode-map (kbd "<f7>") nil)
    (define-key vterm-mode-map (kbd "<f8>") nil)
    (define-key vterm-mode-map (kbd "<f9>") nil)
    (define-key vterm-mode-map (kbd "<f10>") nil)
    (define-key vterm-mode-map (kbd "<f11>") nil)
    (define-key vterm-mode-map (kbd "<f12>") nil)
    ))

;;;;; Theme

(spacemacs/set-leader-keys "Tn" 'my/switch-theme)
(spacemacs/set-leader-keys "TN" 'my/update-my-theme)

;;;;; EKG

(when (locate-library "ekg")
  (global-set-key (kbd "C-c e") 'my/ekg-dispatch)
  (global-set-key (kbd "<f12>") 'ekg-capture)
  ;; (global-set-key (kbd "C-<f12>") 'ash/capture-literature-note)

  ;; or you might prefer a binding like this one
  (with-eval-after-load 'ekg
    (setup-ekg-transients) ; only run this once all ekg funcs are loaded

    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "e") #'my/ekg-notes-dispatch) ; can you think of a better binding for notes-mode?
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "?") #'my/ekg-notes-dispatch) ; help when I'm confused

    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "j") #'ekg-notes-next)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "k") #'ekg-notes-previous)

    ;; TODO merge evil-collection and pull-request
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "A") #'ekg-notes-any-tags)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "a") #'ekg-notes-any-note-tags)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "C") #'ekg-notes-create)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "D") #'ekg-notes-delete)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "r") #'ekg-notes-refresh)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "gr") #'ekg-notes-refresh)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "n") #'ekg-notes-next)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "o") #'ekg-notes-open)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "b") #'ekg-notes-browse)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "B") #'ekg-notes-select-and-browse-url)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "p") #'ekg-notes-previous)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "t") #'ekg-notes-tag)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "q") #'kill-current-buffer)
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "Q") #'kill-buffer-and-window) ; I prefer this behavior
    (evil-define-key '(normal visual) ekg-notes-mode-map (kbd "K") #'ekg-notes-kill) ; dired style
    )
  )
```


#### <span class="section-num">4.2.4</span> Easy Emacs with Mouse {#h:3f7d58fe-d838-4289-849b-c2ce4b3497e0}

```elisp
;;;; Easy Emacs with Mouse

(with-eval-after-load 'calendar
  (define-key calendar-mode-map [(double-mouse-1)] 'org-calendar-goto-agenda)
  )

;;; user-keybindings.el ends here
```


### <span class="section-num">4.3</span> Customs Variables/Faces (`emacs-custom.el`) {#h:4b97497a-5cad-44bc-aa70-df96e910398f}

```elisp
;;; -*- mode: emacs-lisp; coding: utf-8; lexical-binding: t -*-

;; This file is where Emacs writes custom variables.
;; Spacemacs will copy its content to your dotfile automatically in the
;; function `dotspacemacs/emacs-custom-settings'.
;; Do not alter this file, use Emacs customize interface instead.

;;; Custom-set-variables

(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(safe-local-variable-values
   '(
     (org-cite-export-processors
      (t csl "~/org/roam/ieee.csl"))
     (eval add-hook 'after-save-hook
           (lambda nil
             (org-babel-tangle))
           nil t))))

;;; Custom-set-faces

(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(translate-paragraph-highlight ((t (:extend t :background "red"))))
 ;;  '(spell-fu-incorrect-face ((t (:underline (:color "dark violet" :style wave :position nil)))))
 '(pocket-reader-archived ((t (:weight semi-light))))
 ;; '(pocket-reader-unread ((t (:underline t :weight normal))))
 '(wcheck-default-face ((t (:foreground "HotPink1" :underline (:color foreground-color :style wave :position nil)))))
 '(sideline-blame ((t (:foreground "#7a88cf" :background unspecified :height 1.0 :italic t))))
 )
;;; emacs-custom.el ends here
```
