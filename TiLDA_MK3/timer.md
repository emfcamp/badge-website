See the micropython documentation
[1](https://docs.micropython.org/en/latest/pyboard/library/pyb.Timer.html)
for a detailed explanation. Some examples are below.

Note that when your program completes executing, the badge will restart
back to the main menu. If your program doesn't do anything else after
initialising the timer, the badge will look like it just restarts when
you run your program! If you just want the timer callback to keep
running and do nothing else, end your program with a loop like "while
True: pass".

## Function callbacks

    flag = 0

    #note, this callback needs to take one parameter, which is what timer is calling it
    def tim_callback(t):
        global flag
        flag = 1

    timer = pyb.Timer(3)
    timer.init(freq=1)
    timer.callback(tim_callback)

    ....

    timer.deinit()

or, using lambdas

    flag = 0

    #the use of lambda avoids needing the function to take an argument
    def tim_callback():
        global flag
        flag = 1

    timer = pyb.Timer(3)
    timer.init(freq=1)
    timer.callback(lambda t: tim_callback())

    ....

    timer.deinit()