# The LFE REPL

We briefly introduced the REPL in the first version of the Hello-World example we wrote, stating that it was an acronym for 'read-eval-print loop' and how to start it with `rebar3`. As an LFE developer, this is one of the primnary tools -- arguably _the_ most powerful -- at your disposal, so we're going to do a more thorough job of introducing its capabilities in this section.

The first Lisp interpreter was created sometime in late 1958 by then-grad student Steve Russell after reading John McCarthy's definition of `eval`. He had the idea that the theoretical description provided there could actually be implemented in machine code. It is uncertain when that expression of `eval` was first combined with `read` and `print`, though the usefulness of this might not have been very significant until video hardware began to replace teletype machines and punched cards in the 1970s.

A basic REPL can be implemented with just four functions; such an implementation could be started with the following:

```lisp
(LOOP (PRINT (EVAL (READ))))
```

LFE has implemented most these functions for us already (and quite robustly), but we could create our own very limited REPL (single lines with no execution context or environment) within the LFE REPL using the following convenience wrappers:

```lisp
(defun read ()
  (let* ((str (io:get_line "myrepl> "))
         (`#(ok ,expr) (lfe_io:read_string str)))
    expr))

(defun print (result)
  (lfe_io:format "~p~n" `(,result)))

(defun loop (code)
  (loop (print (eval (read)))))
```

Now we can start our custom REPL inside the LFE REPL:

```lisp
lfe> (loop (print (eval (read))))
```

This gives us a new prompt:

```lisp
myrepl>
```

At this prompt we can evaluate basic LFE expressions:

```lisp
myrepl> (+ 1 2 3)
;; 6
myrepl> (* 2 (lists:foldl #'+/2 0 '(1 2 3 4 5 6)))
;; 42
```

Note that writing an evaluator is the hard part, and we've simply re-used the LFE evaluator for this demonstration.

Now that we've explored some of the background of REPLs and Lisp interpreters, let's look more deeply into the LFE REPL and how to best take advantage of its power when using the machine that is LFE and OTP.
