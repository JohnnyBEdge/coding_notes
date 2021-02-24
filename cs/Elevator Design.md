# Elevator Design

## Story

1. User presses up/down button to call elevator.
2. elevator  moves towards user
   1. closest empty elevator heads toward user
   2. full elevator already heading in users direction stops if floor not already passed
   3. if full and passed, continues in direction until empty or end of line. then heads toward user
3. Elevator arrives
4. Doors open
   1. only open when idle
5. User enters
6. User presses floor button
   1. if btn pressed not in direction and no other calls above, change directions
   2. if btn pressed not in direction and other calls already in original direction, continue with original direction before switching
7. doors close after determined time or upon close btn
8. elevator moves to floor
9. Elevator stops
10. doors open
    1. Only when idle
11. user leaves

## Requirements

1. States
   1. direction
      1. up
      2. down
   2. status
      1. idle (assuming if not idle then moving)
   3. current floor
      1. floor number
   4. floor queue
      1. array of floors that have called on the elevator
2. functions
   1. go to floor
   2. open door
   3. close door

## Questions

1. Do idle elevators have place to sit, like back at ground floor or middle of building?
2. Does our building have a basement/floors below ground floor?

## Algorithms

1. Insertion sort
   1. used to order floor selections made by passengers
   2. Floor selection list should be relatively short, making insertion sort fast
   3. assumes passengers getting in elevator are only going in direction of elevator and therefor can be sorted without reversing the direction the elevator