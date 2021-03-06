Monty Hall Problem Simulation
================
[Hendrik Mischo](https://github.com/hendrik-mischo)
27 January 2019

Recently, I watched the movie "21". In one of the scenes they talk about the Monty Hall problem. You can watch this scene here <https://www.youtube.com/watch?v=Zr_xWfThjJ0>.

The Monty Hall problem is a brain teaser, in the form of a probability puzzle, loosely based on the American television game show Let's Make a Deal and named after its original host, Monty Hall:

Suppose you're on a game show, and you're given the choice of three doors: Behind one door is a car; behind the others, goats. You pick a door, say No. 1, and the host, who knows what's behind the doors, opens another door, say No. 3, which has a goat. He then says to you, "Do you want to pick door No. 2?" Is it to your advantage to switch your choice? (source: [Wikipedia.org](https://en.wikipedia.org/wiki/Monty_Hall_problem))

In the movie it is explained that switching to door No. 2 increases your chances of winning to 66.6% rather than 50%.

I was surprised by this, so looked it up. However, I was still in doubt whether this can be correct. I ran the following simulation to find out once and for all.

``` r
set.seed(123)         # Seed for reproducability
n = 10000             # Number of experiments
stay.win.seq = c()    # Empty sequence variable for winning with "stay" strategy
switch.win.seq = c()  # Empty sequence variable for winning with "switch" strategy
doors = c(1,2,3)      # Possible doors

# Run n experiments
for (i in 1:n){
  
  # Randomly select the door behind which there is the car
  car.door = sample(1:3, 1)
  
  # Randomly select on of the doors
  initial.door = sample(1:3, 1)
  
  # Talkshow host reveals one of the doors (but not the selected door or the one where the car is)
  revealed.door = doors[!(doors %in% c(car.door, initial.door))][1]
  
  # Select switched door (not the revealed door or the initially selected one)
  switched.door = doors[!(doors %in% c(revealed.door, initial.door))]
  
  # Add to sequence: TRUE if inital door is where the car is, FALSE otherwise
  stay.win.seq = c(stay.win.seq, ifelse(initial.door == car.door, TRUE, FALSE))
  
  # Add to sequence: TRUE if switched door is where the car is, FALSE otherwise
  switch.win.seq = c(switch.win.seq, ifelse(switched.door == car.door, TRUE, FALSE))
}

# Print the results
cat("Proportion of winnings staying with the initial door: ", paste0(round(mean(stay.win.seq), 3) * 100, "%"), 
    "\n Proportion of winnings switching the door: ", paste0(round(mean(switch.win.seq), 3) * 100, "%"))
```

    Proportion of winnings staying with the initial door:  33.5% 
     Proportion of winnings switching the door:  66.5%

Counterintuitively, it seems to be true that switching the door after the talk show host has revealed one where the car is not, increases the chances of winning.
