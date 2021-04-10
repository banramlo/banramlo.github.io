---
layout: post
title: Monty hall problem and monte carlo method
gh-repo: daattali/beautiful-jekyll
tags: [simulation, approximation]
comments: true
use_math: true
---
<div align="center">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/iBdjqtR2iK4?start=66" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

There is a famous problem known as "Monty hall problem".
There are three doors.
One of the door has a car and others have a goat.
If a person select a door, host will open one door with a goat.
Now, person got one chance to move the selection or not.
So the question is which one is better in between move or not?
In a mathmatics aspect, it is proven that moving the selection makes better decision.

It can be easily extended to $n$ door problem.
Let's assume that $A$ is an event that chooses the right door at the first trial which $P(A) = \frac{1}{n}$.
If a person choosed the right door, then change makes selection wrong.
If a person choosed the wrong door, then change may make the right selection between $n - 2$ doors.
As a result, if a person changes a door it will be $\frac{1}{n} \times 0 + \frac{n - 1}{n} \times \frac{1}{n-2} = \frac{n-1}{n \times (n-2)}$.

Which means it results in better probability to win.

<div align="center" id="sortResultPlot1">
</div>

<script>
    var Before = {name: 'Don\'t change the choice', type: 'scatter', x: [],y: []};
    var After = {name: 'Change the choice', type: 'scatter', x: [],y: []};
    for(var i = 3; i < 11; ++i){
        Before['x'].push(i);
        Before['y'].push(1/i);
        After['x'].push(i);
        After['y'].push((i - 1)/(i * (i - 2)));
    }
    var data = [Before, After];
    var layout = {
        xaxis: {
            title: '# doors',
            showgrid: false,
            zeroline: false
        },
        yaxis: {
            title: 'Win probability',
            showline: false
        }
    };
    Plotly.newPlot('sortResultPlot1', data, layout);
</script>


It can simulated by code below.

```c
#include <time.h>
#include <iostream>

int main(){
    using namespace std;

    int numDoor;
    int numIteration;
    int success = 0;
    int failed = 0;
    int price, selected, changed;
    srand(time(NULL));

    cout << ("Enter the trial number: ");
    cin >> numIteration;
    cout << ("Enter the number of doors: ");
    cin >> numDoor;

    bool door[numDoor];
    for (int i = 0; i < numIteration; i++){
        for (int j = 0; j < numDoor; j++){
            door[j] = false;
        }

        price = rand() % numDoor;
        door[price] = true;

        selected = rand() % numDoor;

        int hostSelected = rand() % numDoor;
        while(hostSelected == selected || door[hostSelected]){
            hostSelected = rand() % numDoor;
        }

        changed = rand() % numDoor;
        while((changed == selected) || (changed == hostSelected)){
            changed = rand() % numDoor;
        }

        if (door[changed]){
            success++;
        }
        else{
            failed++;
        }
    }
    cout << "# success " << success << ", # failed " << failed << ", Percentage " << success / float(numIteration) * 100 << std::endl;
    return 0;
}
```

This is close to monte-carlo method.
It just tries the change and check wheter it is price or not.
Collect the data and guess the actual probability.

| # trial | # door | # success | # fail | Probability(From experiment) | Probability(From theory)|
| :------ |:--- | :--- | :--- | :--- | :--- |
| 10000 | 3 | 6703 | 3297 | 67.03 | $\frac{2}{3} \times \frac{1}{1} \times 100 = 66.\dot{6}$ |
| 10000 | 4 | 3768 | 6232 | 37.68 | $\frac{3}{4} \times \frac{1}{2} \times 100 = 37.5$ |
| 10000 | 5 | 2608 | 7392 | 26.08 | $\frac{4}{5} \times \frac{1}{3} \times 100 = 26.\dot{6}$ |
| 10000 | 6 | 2028 | 7972 | 20.28 | $\frac{5}{6} \times \frac{1}{4} \times 100 = 20.8\dot{3}$ |

Here is one more example to compute the area of circle.

```c
#include <iostream>
#include <time.h>

int main(){
    using namespace std;
    constexpr int numIteration = 1000000;
    srand(time(NULL));

    int inSide = 0;
    for(int i = 0; i < numIteration; ++i){
        float x = rand() % 10000 / float(5000) - 1; // -1 ~ 1
        float y = rand() % 10000 / float(5000) - 1; // -1 ~ 1
        if(x * x + y * y <= 1){
            ++inSide;
        }
    }
    cout << "Inside of circle : " << inSide << ", circle area may be " << 4 * inSide/float(numIteration) << std::endl;
}
```

It is known to be $\pi r^2$.
Circle area is $3.14082$ with $r = 1$ from the experiment.
Which means it works pretty well.
