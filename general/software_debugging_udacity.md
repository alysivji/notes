# CS259 Software Debugging

[Course Link](https://classroom.udacity.com/courses/cs259/)
[Course Wiki](https://www.udacity.com/wiki/cs259)

The notes below are a translation of Chris McGinlay's [Summary Notes](https://docs.google.com/document/pub?id=1jesnYtfsVsnNPA3rSwmMC6vEh3k-F099b9tCD0UTl_I) into Markdown.

Thanks to Chris doing all the hard work and adding a [Creative Commons license](https://en.wikipedia.org/wiki/Creative_Commons_license) to his notes.

## Lesson 1: How Debuggers Work

* Idea of debugging by conducting a systematic experiment to establish cause(s) of failure.

```text
Input ->
    Failed Program ->
        Compare expected and actual outputs for given inputs ->
            Find defect causing failure
```

### Devil’s Guide to Debugging aka Debugging DONTs

* Scatter output statements everywhere. (“print” or log entry). These waste time, have to enter statements, read and decipher horrible output, remove statements. Security issues if they are left in production code (eg. read passwords in clear in log file).
* Just make it work. Don’t bother to understand it. (Causes unintended breakages later).
* Never back up earlier working versions.
* Don’t bother to understand what the code is meant to do
Fix the symptom not the problem.
* Apply the most obvious fix.

### Definitions

**defect** - An error in code. Preferable to the term ‘bug’ which implies that something crawled in when you weren’t looking, whereas in fact the programmer put the defect there in the first place.

**failure** - an externally visible error.

**error** a deviation from correct behaviour.

**infection** error in program state, propagating forward to successive states.

**program**  succession of states represented by a state vector (of variables, objects etc)

A defective statement causes a change of state, such that a sane input state becomes an insane state. Marked by an infection with no precursor infection.

Possible: defect -> infection -> failure
Sometimes: defect -> infection -> no failure

However a failure can always be traced back to a defect via cause-effect chain. This can be HARD; tens of thousands of variables, tens of thousands of intermediate states.

### Scientific Method

Posit a hypothesis. (Something you expect to happen, or think may be happening to program state). Devise an experiment to test this hypothesis. E.g. use ‘assert False’ to mark code which should never be reached (such as an undefined branch of if..elif..else).

Hypotheses -> Prediction -> Experiment -> Observation.

Observation will result in either confirmation or rejection of hypothesis. So either you refine or reject your hypothesis. Finally, if you have established cause of defect ensure you:
1. prevent re-occurrence of defect
1. look for same essential error elsewhere in code

### Explicit Debugging...

(...as opposed to keeping the whole situation in your head)

Keep a written log of what you’ve tried. No need to memorise. Easy to interrupt work and resume later. The act of writing down structures forces thinking to take place and can reveal solution. Similarly, talk to someone about the problem. Act of explaining forces you to understand the situation and structure your thoughts prior to opening your mouth.

### Code from Lecture

```python
#!/usr/bin/env python
# Simple debugger
import sys
import readline

def remove_html_markup(s):
    tag   = False
    quote = False
    out   = ""

    for c in s:
        if c == '<' and not quote:
            tag = True
        elif c == '>' and not quote:
            tag = False
        elif c == '"' or c == "'" and tag:
            quote = not quote
        elif not tag:
            out = out + c
    return out

# main program that runs the buggy program
def main():
    print remove_html_markup('xyz')
    print remove_html_markup('"<b>foo</b>"')
    print remove_html_markup("'<b>foo</b>'")

# globals
breakpoints = {9: True, 14: True}
stepping = False

def debug(command, my_locals):
    global stepping
    global breakpoints

    if command.find(' ') > 0:
        arg = command.split(' ')[1]
    else:
        arg = None

    if command.startswith('s'):     # step
        stepping = True
        return True
    elif command.startswith('c'):   # continue
        stepping = False
        return True
    elif command.startswith('q'):   # quit
        sys.exit(0)
    else:
        print "No such command", repr(command)

    return False

commands = ["s", "s", "q"]

def input_command():
    #command = raw_input("(my-spyder) ")
    global commands
    command = commands.pop(0)
    return command

def traceit(frame, event, trace_arg):
    global stepping

    if event == 'line':
        if stepping or breakpoints.has_key(frame.f_lineno):
            resume = False
            while not resume:
                print event, frame.f_lineno, frame.f_code.co_name, frame.f_locals
                command = input_command()
                resume = debug(command, frame.f_locals)
    return traceit

sys.settrace(traceit)

print remove_html_markup('xyz')
```

Add more features to the debuggers.
