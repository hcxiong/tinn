![](img/logo.PNG)

Tinn (Tiny Neural Network) is a 200 line dependency free neural network library written in C99.

    #include "Tinn.h"
    #include <stdio.h>

    #define SETS 4
    #define NIPS 2
    #define NHID 8
    #define NOPS 1
    #define ITER 2000
    #define RATE 1.0f

    int main()
    {
        float in[SETS][NIPS] = {
            { 0, 0 },
            { 0, 1 },
            { 1, 0 },
            { 1, 1 },
        };
        float tg[SETS][NOPS] = {
            { 0 },
            { 1 },
            { 1 },
            { 0 },
        };
        // Build.
        const Tinn tinn = xtbuild(NIPS, NHID, NOPS);
        // Train.
        for(int i = 0; i < ITER; i++)
        {
            float error = 0.0f;
            for(int j = 0; j < SETS; j++)
                error += xttrain(tinn, in[j], tg[j], RATE);
            printf("%.12f\n", error / SETS);
        }
        // Predict.
        for(int i = 0; i < SETS; i++)
        {
            const float* pd = xtpredict(tinn, in[i]);
            printf("%f :: %f\n", tg[i][0], (double) pd[0]);
        }
        // Cleanup.
        xtfree(tinn);
        return 0;
    }

For a quick demo, get some training data:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/semeion/semeion.data

And if you're on Linux / MacOS just build and run:

    make; ./tinn

If you're on Windows it's:

    mingw32-make & tinn.exe

The training data consists of hand written digits written both slowly and quickly.
Each line in the data set corresponds to one handwritten digit. Each digit is 16x16 pixels in size
giving 256 inputs to the neural network.

At the end of the line 10 digits signify the digit:

    0: 1 0 0 0 0 0 0 0 0 0
    1: 0 1 0 0 0 0 0 0 0 0
    2: 0 0 1 0 0 0 0 0 0 0
    3: 0 0 0 1 0 0 0 0 0 0
    4: 0 0 0 0 1 0 0 0 0 0
    ...
    9: 0 0 0 0 0 0 0 0 0 1

This gives 10 outputs to the neural network. The test program will output the
accuracy for each digit. Expect above 99% accuracy for the correct digit, and
less that 1% accuracy for the other digits.

# Disclaimer

Tinn is not a fully featured neural network C library like Kann, or Genann:

    https://github.com/attractivechaos/kann

    https://github.com/codeplea/genann

Tinn is minimal and easy to digest.
