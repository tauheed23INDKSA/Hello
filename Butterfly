# Hello World
My first repository on GitHub: Butterfly![butterfly](https://github.com/user-attachments/assets/ee2fcb14-4e11-4903-a184-a18b43106a29)

clear; clc; close all

a = 0.1;
f = 5;
k = 0.1;
fps = 30;
Tend = 6;

Ny  = 220;
Nth = 260;

yminB = -pi/2 + 1e-3;
ymaxB =  pi/2 - 1e-3;

yB  = linspace(yminB, ymaxB, Ny);
th  = linspace(0, 2*pi, Nth);
[TH, Ybody] = meshgrid(th, yB);

Rbody = sqrt(max(k*cos(Ybody), 0));
Xbody = Rbody .* cos(TH);
Zbody = Rbody .* sin(TH);

NxW = 600; 
NyW = 450;
b = 1; 
c = 1;

xW = linspace(-4, 4, NxW);
yW = linspace(-3, 3, NyW);
[Xw, Yw] = meshgrid(xW, yW);

I1 = abs(Yw) < (1.5 + log(abs(b*Xw) + eps));
I2 = abs(Xw) < (2 + sin(c*Yw));
maskW = I1 & I2;

fig = figure('Color','w');
ax = axes('Parent',fig);
hold(ax,'on');
grid(ax,'on');
axis(ax,'equal');
xlabel(ax,'x');
ylabel(ax,'y');
zlabel(ax,'z');
view(ax, [-35 20]);
camlight(ax,'headlight');
lighting(ax,'gouraud');

surf(ax, Xbody, Ybody, Zbody, 'EdgeColor','none', 'FaceAlpha',0.95);

t = 0;
Zw = a * sin(f*t) .* (Xw.^2) .* maskW;
Zw(~maskW) = NaN;

hWing = surf(ax, Xw, Yw, Zw, 'EdgeColor','none', 'FaceAlpha',0.95);

xlim(ax, [min(xW) max(xW)]);
ylim(ax, [min(yW) max(yW)]);
zlim(ax, [-max(sqrt(k), a*(max(abs(xW))^2))  max(sqrt(k), a*(max(abs(xW))^2))]);

gifFile = 'butterfly.gif';
dt = 1/fps;
frameCount = 0;

while ishandle(fig) && t <= Tend
    Zw = a * sin(f*t) .* (Xw.^2) .* maskW;
    Zw(~maskW) = NaN;

    set(hWing, 'ZData', Zw, 'CData', Zw);
    drawnow;

    frame = getframe(fig);
    im = frame2im(frame);
    [imind, cm] = rgb2ind(im, 256);

    if frameCount == 0
        imwrite(imind, cm, gifFile, 'gif', 'Loopcount', inf, 'DelayTime', dt);
    else
        imwrite(imind, cm, gifFile, 'gif', 'WriteMode', 'append', 'DelayTime', dt);
    end

    frameCount = frameCount + 1;
    t = t + dt;
end
