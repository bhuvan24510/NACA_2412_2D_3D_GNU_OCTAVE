**NACA 2412 Airfoil — Geometric Modelling in 2D and 3D**
A parametric geometric model of the NACA 2412 airfoil in both two and three dimensions, implemented in GNU Octave and Python (NumPy + Matplotlib) so that each implementation can verify the other.
**Overview**
The airfoil is built from the standard NACA four-digit equations:
• A piecewise parabolic mean camber line
• An analytic thickness distribution
• Thickness applied perpendicular to the camber line to obtain upper and lower surfaces
The 2D programs plot the airfoil outline, camber distribution and thickness distribution. The 3D programs extrude the section along the span to produce a straight rectangular wing. All figures are exported as 300 dpi images suitable for reports and as a starting point for CFD pre-processing. The NACA 2412 is the airfoil of the Cessna 172, making it a convenient and recognisable reference case for teaching and for validating geometry tools.
**NACA 2412 Nomenclature**
A NACA four-digit airfoil is named MPXX:
Digit(s) - Meaning - Value for 2412
M (first) - Maximum camber, in percent of chord - 2 %, so m = 0.02
P (second) - Position of maximum camber, in tenths of chord - 4, so p = 0.40
XX (last two) - Maximum thickness, in percent of chord - 12 %, so t = 0.12
The NACA 2412 therefore has 2 % camber at 40 % chord and a maximum thickness of 12 % chord.
**Mathematical Formulation**
All lengths are normalised by the chord, with x running from 0 at the leading edge to 1 at the trailing edge.
**Mean camber line**
The camber line is two parabolic arcs meeting at x = p.
For 0 ≤ x ≤ p:
yc = (m / p²) * (2*p*x − x²)
dyc/dx = (2*m / p²) * (p − x)
For p < x ≤ 1:
yc = (m / (1−p)²) * (1 − 2*p + 2*p*x − x²)
dyc/dx = (2*m / (1−p)²) * (p − x)
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 1
NACA 2412 Airfoil — Geometric
Modelling in 2D and 3D
Author: Bhuvaneshwari N
Department: Aeronautical Engineering, Dhanalakshmi Srinivasan College of Engineering and
Technology
A parametric geometric model of the NACA 2412 airfoil in both two and three dimensions,
implemented in GNU Octave and Python (NumPy + Matplotlib) so that each implementation can
verify the other.
Overview
The airfoil is built from the standard NACA four-digit equations:
• A piecewise parabolic mean camber line
• An analytic thickness distribution
• Thickness applied perpendicular to the camber line to obtain upper and lower surfaces
The 2D programs plot the airfoil outline, camber distribution and thickness distribution. The 3D
programs extrude the section along the span to produce a straight rectangular wing. All figures are
exported as 300 dpi images suitable for reports and as a starting point for CFD pre-processing.
The NACA 2412 is the airfoil of the Cessna 172, making it a convenient and recognisable reference
case for teaching and for validating geometry tools.
NACA 2412 Nomenclature
A NACA four-digit airfoil is named MPXX:
Digit(s) Meaning Value for 2412
M (first) Maximum camber, in percent of chord 2 %, so m = 0.02
P (second) Position of maximum camber, in tenths of chord 4, so p = 0.40
XX (last two) Maximum thickness, in percent of chord 12 %, so t = 0.12
The NACA 2412 therefore has 2 % camber at 40 % chord and a maximum thickness of 12 % chord.
Mathematical Formulation
All lengths are normalised by the chord, with x running from 0 at the leading edge to 1 at the trailing
edge.
Mean camber line
The camber line is two parabolic arcs meeting at x = p.
For 0 ≤ x ≤ p:
yc = (m / p²) * (2*p*x − x²)
dyc/dx = (2*m / p²) * (p − x)
For p < x ≤ 1:
yc = (m / (1−p)²) * (1 − 2*p + 2*p*x − x²)
dyc/dx = (2*m / (1−p)²) * (p − x)
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 2
Thickness distribution
The half-thickness (distance from camber line to each surface, before rotation) is:
yt = 5*t * ( 0.2969*√x − 0.1260*x − 0.3516*x² + 0.2843*x³ − 0.1015*x⁴ )
The total thickness at a station is 2*yt — the quantity plotted in the thickness figure.
Upper and lower surfaces
The local camber angle is θ = atan(dyc/dx). Thickness is applied perpendicular to the camber line:
Upper: xu = x − yt*sin(θ), yu = yc + yt*cos(θ)
Lower: xl = x + yt*sin(θ), yl = yc − yt*cos(θ)
Multiplying all coordinates by the chord c gives dimensional values in metres.
Extension to 3D
The 3D wing is produced by extruding the 2D section along the span direction z from 0 to b.
Meshgrid operations replicate the upper and lower surface coordinates at every spanwise station,
giving a straight, untwisted, untapered (rectangular) wing.
Input Parameters
Parameter Symbol Value
Maximum camber m 0.02 (2 % chord)
Camber position p 0.40 (40 % chord)
Maximum thickness t 0.12 (12 % chord)
Chord length c 1.0 m
Wing span b 2.0 m
Chordwise points Nx 201 (2D), 200 (3D in the Octave script)
Spanwise points Nz 50
Repository Contents
File Description
NACA2412.m GNU Octave 2D script — outline, camber, thickness plots
NACA3D2412.m GNU Octave 3D script — extruded rectangular wing
naca_airfoil.py Python 2D + 3D script (Google Colab compatible), generic for any NACA four-digit
code
README.md This file
Outputs generated
• NACA2412_Geometry.png — airfoil outline with mean camber line
• NACA2412_Camber.png — camber distribution
• NACA2412_Thickness.png — thickness distribution
• NACA2412_3D_Airfoil.png — 3D extruded wing
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 3
Implementation
GNU Octave (2D) — NACA2412.m
clc; clear; close all;
% Airfoil parameters
m = 0.02; % Maximum camber
p = 0.40; % Position of maximum camber
t = 0.12; % Maximum thickness
c = 1.0; % Chord length in metres
x = linspace(0, 1, 201);
yc = zeros(size(x));
dyc = zeros(size(x));
% Camber line and slope
for i = 1:length(x)
 if x(i) <= p
 yc(i) = (m/p^2) * (2*p*x(i) - x(i)^2);
 dyc(i) = (2*m/p^2) * (p - x(i));
 else
 yc(i) = (m/(1-p)^2) * (1 - 2*p + 2*p*x(i) - x(i)^2);
 dyc(i) = (2*m/(1-p)^2) * (p - x(i));
 endif
endfor
% Thickness distribution
yt = 5*t*( 0.2969*sqrt(x) - 0.1260*x - 0.3516*x.^2 ...
 + 0.2843*x.^3 - 0.1015*x.^4 );
theta = atan(dyc);
% Upper and lower surfaces
xu = x - yt.*sin(theta); yu = yc + yt.*cos(theta);
xl = x + yt.*sin(theta); yl = yc - yt.*cos(theta);
% Figure 1: Airfoil geometry
figure(1);
plot(xu*c, yu*c, 'b', 'LineWidth', 2); hold on;
plot(xl*c, yl*c, 'r', 'LineWidth', 2);
plot(x*c, yc*c, 'k--', 'LineWidth', 1.5);
grid on; axis equal;
xlabel('Chord Position (m)'); ylabel('Vertical Position (m)');
title('NACA 2412 Airfoil Geometry');
legend('Upper Surface','Lower Surface','Mean Camber Line');
print(1, 'NACA2412_Geometry.png', '-dpng', '-r300');
% Figure 2: Camber distribution
figure(2);
plot(x, yc, 'LineWidth', 2); grid on;
xlabel('Normalized Chord (x/c)'); ylabel('Normalized Camber (yc/c)');
title('NACA 2412 Camber Distribution');
print(2, 'NACA2412_Camber.png', '-dpng', '-r300');
% Figure 3: Thickness distribution
figure(3);
plot(x, 2*yt, 'LineWidth', 2); grid on;
xlabel('Normalized Chord (x/c)'); ylabel('Normalized Thickness (t/c)');
title('NACA 2412 Thickness Distribution');
print(3, 'NACA2412_Thickness.png', '-dpng', '-r300');
% Display results
fprintf('\nNACA 2412 AIRFOIL RESULTS\n');
fprintf('Maximum Camber: %.2f %%\n', m*100);
fprintf('Camber Position: %.2f %%\n', p*100);
fprintf('Maximum Thickness: %.2f %%\n', t*100);
fprintf('Chord Length: %.2f m\n', c);
GNU Octave (3D) — NACA3D2412.m
Uses the same geometry calculation, then builds coordinate matrices with meshgrid and draws the
upper and lower surfaces with surf. View set with view(35,25) and axis equal.
clc; clear; close all;
% Airfoil parameters
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 4
m = 0.02; p = 0.40; t = 0.12;
c = 1.0; b = 2.0;
Nx = 200; Nz = 50;
x = linspace(0,1,Nx);
z = linspace(0,b,Nz);
yc = zeros(size(x)); dyc = zeros(size(x));
for i = 1:length(x)
 if x(i) <= p
 yc(i) = (m/p^2)*(2*p*x(i) - x(i)^2);
 dyc(i) = (2*m/p^2)*(p - x(i));
 else
 yc(i) = (m/(1-p)^2)*(1 - 2*p + 2*p*x(i) - x(i)^2);
 dyc(i) = (2*m/(1-p)^2)*(p - x(i));
 endif
endfor
yt = 5*t*( 0.2969*sqrt(x) - 0.1260*x - 0.3516*x.^2 ...
 + 0.2843*x.^3 - 0.1015*x.^4 );
theta = atan(dyc);
xu = x - yt.*sin(theta); yu = yc + yt.*cos(theta);
xl = x + yt.*sin(theta); yl = yc - yt.*cos(theta);
% 3D coordinate matrices
[XU, ZU] = meshgrid(xu*c, z); [YU, ~] = meshgrid(yu*c, z);
[XL, ZL] = meshgrid(xl*c, z); [YL, ~] = meshgrid(yl*c, z);
figure(1);
surf(XU, YU, ZU); hold on;
surf(XL, YL, ZL);
shading interp; axis equal; grid on;
xlabel('Chord Direction X (m)');
ylabel('Vertical Direction Y (m)');
zlabel('Span Direction Z (m)');
title('3D NACA 2412 Airfoil');
view(35,25);
print(1,'NACA2412_3D_Airfoil.png','-dpng','-r300');
Python (Google Colab) — naca_airfoil.py
The Python program reproduces the same equations using NumPy. The loop of the Octave version is
replaced by a vectorised np.where. Matplotlib produces all four figures.
The airfoil is specified as a string (e.g. naca = "2412"), and m, p, t are derived from it:
m = int(naca[0]) / 100
p = int(naca[1]) / 10
t = int(naca[2:]) / 100
Changing that single line regenerates all figures for any four-digit section. Titles and file names
update automatically.
import numpy as np
import matplotlib.pyplot as plt
# INPUTS
naca = "2412" # NACA 4-digit code
c = 1.0 # Chord length (m)
b = 2.0 # Wing span (m)
Nx, Nz = 201, 50 # Points along chord / span
m = int(naca[0]) / 100
p = int(naca[1]) / 10
t = int(naca[2:]) / 100
# AIRFOIL GEOMETRY
x = np.linspace(0, 1, Nx)
z = np.linspace(0, b, Nz)
yc = np.where(x <= p,
 (m/p**2) * (2*p*x - x**2),
 (m/(1-p)**2) * (1 - 2*p + 2*p*x - x**2))
dyc = np.where(x <= p,
 (2*m/p**2) * (p - x),
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 5
 (2*m/(1-p)**2) * (p - x))
yt = 5*t*(0.2969*np.sqrt(x) - 0.1260*x - 0.3516*x**2
 + 0.2843*x**3 - 0.1015*x**4)
theta = np.arctan(dyc)
xu = (x - yt*np.sin(theta)) * c; yu = (yc + yt*np.cos(theta)) * c
xl = (x + yt*np.sin(theta)) * c; yl = (yc - yt*np.cos(theta)) * c
# FIGURE 1: 2D AIRFOIL GEOMETRY
plt.figure(figsize=(8, 4))
plt.plot(xu, yu, 'b', lw=2, label='Upper Surface')
plt.plot(xl, yl, 'r', lw=2, label='Lower Surface')
plt.plot(x*c, yc*c, 'k--', lw=1.5, label='Mean Camber Line')
plt.axis('equal'); plt.grid(alpha=0.4)
plt.xlabel('Chord Position (m)'); plt.ylabel('Vertical Position (m)')
plt.title(f'NACA {naca} Airfoil Geometry')
plt.legend()
plt.savefig(f'NACA{naca}_Geometry.png', dpi=300, bbox_inches='tight')
plt.show()
# FIGURE 2: CAMBER DISTRIBUTION
plt.figure(figsize=(7, 4))
plt.plot(x, yc, lw=2); plt.grid(alpha=0.4)
plt.xlabel('Normalized Chord (x/c)'); plt.ylabel('Normalized Camber (yc/c)')
plt.title(f'NACA {naca} Camber Distribution')
plt.savefig(f'NACA{naca}_Camber.png', dpi=300, bbox_inches='tight')
plt.show()
# FIGURE 3: THICKNESS DISTRIBUTION
plt.figure(figsize=(7, 4))
plt.plot(x, 2*yt, lw=2); plt.grid(alpha=0.4)
plt.xlabel('Normalized Chord (x/c)'); plt.ylabel('Normalized Thickness (t/c)')
plt.title(f'NACA {naca} Thickness Distribution')
plt.savefig(f'NACA{naca}_Thickness.png', dpi=300, bbox_inches='tight')
plt.show()
# FIGURE 4: 3D WING
XU, ZU = np.meshgrid(xu, z); YU, _ = np.meshgrid(yu, z)
XL, ZL = np.meshgrid(xl, z); YL, _ = np.meshgrid(yl, z)
fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')
ax.plot_surface(XU, ZU, YU, color='steelblue', alpha=0.9, linewidth=0)
ax.plot_surface(XL, ZL, YL, color='indianred', alpha=0.9, linewidth=0)
ax.set_xlabel('Chord Direction X (m)')
ax.set_ylabel('Span Direction Z (m)')
ax.set_zlabel('Vertical Direction Y (m)')
ax.set_title(f'3D NACA {naca} Airfoil')
ax.set_box_aspect((c, b, 0.25*c))
ax.view_init(elev=25, azim=-55)
plt.savefig(f'NACA{naca}_3D.png', dpi=300, bbox_inches='tight')
plt.show()
# RESULTS
print(f'\nNACA {naca} AIRFOIL RESULTS')
print(f'Maximum Camber: {m*100:.2f} %')
print(f'Camber Position: {p*100:.2f} % chord')
print(f'Maximum Thickness: {t*100:.2f} %')
print(f'Chord Length: {c:.2f} m')
print(f'Wing Span: {b:.2f} m')
print('\nAll 4 figures saved successfully.')
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 6
Octave vs Python — Key Differences
Aspect Octave Python
Syntax Loops with endif / endfor, comments with
%
Vectorised np.where, comments with #
3D plotting surf(X, Y, Z) with Z as span plot_surface(X, Z, Y) so Y stays vertical
Equal axes axis equal set_box_aspect((c, b, 0.25*c))
Airfoil selection Edit m, p, t by hand Single NACA code string
Environment Desktop Octave Browser, no installation
Because Matplotlib treats the third argument as the vertical axis, the Python code passes arrays in
the order (X, Z, Y). The Octave 3D script keeps the order (X, Y, Z), so its view orientation differs even
though the geometry is identical.
Results
Quantity Value
Maximum camber 2.00 % of chord (0.0200 m for c = 1 m)
Location of maximum camber 40 % chord
Maximum thickness 12.00 % of chord (0.1200 m for c = 1 m)
Location of maximum thickness approximately 30 % chord
Leading-edge radius (1.1019 t²) about 0.0159 c
Chord / span 1.0 m / 2.0 m
• Figure 1 — Airfoil geometry: rounded leading edge, upper surface bulging above the camber
line, lower surface flatter and closer to it, sharp trailing edge. The dashed mean camber line lies
between the two surfaces everywhere.
• Figure 2 — Camber distribution: rises smoothly from zero at the leading edge to a peak of
0.02 at x/c = 0.4, then returns to zero at the trailing edge. Check: at x = 0.2 the equation gives yc
= 0.015, matching the plot.
• Figure 3 — Thickness distribution: total thickness rises steeply from the leading edge, peaks
at 0.12 near x/c = 0.30, and tapers toward the trailing edge. The √x term produces the rounded
nose.
• Figure 4 — 3D wing: the extruded surface shows the cambered rectangular wing of 1 m chord
and 2 m span. Upper and lower skins are drawn in different colours so the camber is visible.
Verification note: the maximum of the thickness plot must equal the last two digits of the NACA
code divided by 100. For the 2412 the peak must be 0.12.
Agreement between Octave and Python: both programs use identical equations and
parameters, producing the same outline, camber curve and thickness curve. Any visible difference is
in styling and 3D view orientation only.
Limitations
• Trailing edge is not perfectly closed. With the standard coefficient 0.1015 on the x⁴ term, the
polynomial gives a small finite thickness at x = 1 (≈ 0.0025 c in total). Using 0.1036 instead
closes it exactly — usually preferred for structured CFD meshes.
NACA 2412 Airfoil — Geometric Modelling in 2D and 3D Page 7
• Uniform chordwise spacing. Points are evenly spaced in x. Because the surface slope is very
large at the nose, cosine spacing, x = 0.5·(1 − cos(πs)) for s from 0 to 1, gives a smoother leading
edge with fewer points.
• Open wing tip. The 3D surface has no end caps, so it is a surface model rather than a closed
solid.
• Simple planform. No taper, sweep, twist or dihedral.
• Geometry only. No aerodynamic loads or coefficients are computed.
Conclusion
A parametric NACA 2412 model was created in both GNU Octave and Python. The programs
generate the 2D outline, camber and thickness distributions and a 3D wing, and export all of them
as high-resolution images. The two implementations agree, and the Python version can generate
any NACA four-digit airfoil from a single input. The model is a reliable geometric base for further
aerodynamic work.
Future Scope
• Switch to cosine point spacing and a closed trailing edge for mesh generation.
• Export the coordinates to a .dat file for use in Gmsh, XFOIL or other CFD preprocessors.
• Add taper, sweep and twist to the 3D wing.
• Add end caps to create a closed solid for CAD (STL) export.
• Add NACA five-digit and six-digit series.
• Couple the geometry with a panel method or CFD solver to compute lift, drag and pressure
distribution.
References
1. Abbott, I. H. and von Doenhoff, A. E., Theory of Wing Sections, Dover Publications, 1959.
2. Anderson, J. D., Fundamentals of Aerodynamics, McGraw-Hill.
3. NASA Langley Research Center, The NACA Airfoil Series
(turbmodels.larc.nasa.gov/naca4412_val.html and related pages).
4. GNU Octave documentation, www.octave.org.
5. NumPy and Matplotlib documentation, numpy.org and matplotlib.org.
How to Run
GNU Octave:
octave NACA2412.m # 2D figures
octave NACA3D2412.m # 3D wing
Python:
pip install numpy matplotlib
python naca_airfoil.py
Or open in Google Colab — no installation required. Change naca = "2412" to any four-digit code
(e.g. "0012", "4415") and re-run to regenerate all figures.
