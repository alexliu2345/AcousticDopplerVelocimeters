## AcousticDopplerVelocimeters

Acoustic Dopper Velocimeters (ADVs) are standard oceanographic instru- ments for measuring water velocity at high frequency, usually between 4 - 100 Hz. These measurements allow researchers to study turbulence in the ocean, which is important for understanding ocean physics and the ocean’s role in the Earth’s climate.
ADVs work by sending sound waves into the water and then listening to the return echo after the sound waves bounce off of suspended particles in the water. By using the Doppler shift between subsequent pulses (i.e., the change in frequency of the returned sound due to the particle moving with the flow), ADVs can estimate the velocity of the water.

In adv.csv you will find a 14-minute sample of ADV data that Prof. Galen Egan collected in South San Francisco Bay in July, 2018. The time column is in units of seconds starting at 0. The U column provides the water speed in m/s (we won’t worry about flow direction in this assignment). The goal is to clean the ADV data while practicing:
• Object-oriented programming
• Data visualization
• Outlier identification
• Statistics and analytic geometry 
• Programming flow control

Goring-Nikora algorithm implementation:


# 1. Calculate estimates of the first and second derivative of the velocity
∆ui = (ui+1 − ui−1)/2 (1) ∆2ui = (∆ui+1 − ∆ui−1)/2 (2)
where the subscript i denotes the ith element of the vector u. You may find the np.gradient function helpful here.
# 2. Calculate the standard deviation of velocity and its first two derivatives,
 σu, σ∆u, and σ∆2u.
# 3. Calculate the universal maximum scaling factor λ =√(2ln(n))
 where n is the number of velocity observations. You can think of this as the maximum number of standard deviations of variability you can expect to observe for a Gaussian process sampled n times.
# 4. Calculate the rotation angle θ for the u-∆2u ellipse as
θ = tan^-1(sum((ui*d^2ui)/sum(ui^2))
# 5. Calculate the major and minor axes of each of the three ellipses as
follows:
(a) u-∆u: define the major axis a1 = λσu, and the minor axis b1 =
λσ∆u
(b) u-∆2u: Calculate the major and minor axes a2 and b2 as the solutions to the set of equations
  (λσ )^2 =a^2cos^2θ+b^2sin^2θ
  (λσ^2)^2 =a^2sin^2θ+b^2cos^2θ 
(c) ∆u-∆2u: define the major axis a3 = λσ∆u and the minor axis b3 = λσ∆2u
# 6. Find the union of all “bad” indices where any of the pairs (u,∆u), (u,∆2u), or (∆u,∆2u) fall outside of the bounds of their respective ellipses (e.g., Figure 4b in Goring & Nikora). This corresponds to any data pair (x, y) that satisfies the condition:
(cos θ(x − x) + sin θ(y − y))^2/a^2 +(sin θ(x − x) − cos θ(y − y))^2/b^2 >1
where x is the major axis variable, y is the minor axis variable, θ is the
rotation of the major axis, and a bar () denotes the average value.
(a) For the u-∆u ellipse, u is on the major axis, ∆u is on the minor
axis, and the ellipse is not rotated (θ = 0)
(b) For the u-∆2u ellipse, u is on the major axis, ∆2u is on the minor
axis, and the ellipse is rotated by θ
(c) For the ∆u-∆2u ellipse, ∆u is on the major axis, ∆2u is on the minor axis, and the ellipse is not rotated (θ = 0).

# 7. Set velocity to np.nan at any of the bad indices, and then impute over the NaN value using a method of your choosing (e.g., linear interpola- tion, nearest neighbor interpolation, replacement by a moving average value, etc.).
# 8. Repeat steps 1 - 7 until the total number of bad indices is less than 5.
