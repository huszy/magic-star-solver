# magic-star-solver
Magic Start solver MATLAB algorithm 
This script solves the star placement below with the following numbers required to put in the empty places: 9,6,10,5,8,4,12,6,8.

Original puzzle:

![original](https://github.com/huszy/magic-star-solver/assets/614902/9a6c3536-537a-4181-b357-03a185528d08)

Possible solution:

![solved](https://github.com/huszy/magic-star-solver/assets/614902/7c3064ca-eed6-4ea1-b58b-4ac6552ce9a1)

```MATLAB
calculateStar();

function calculateStar
    % number of star points
    n = 6;
    S = sum(1:2*n);
    % sum of star points
    L = 2/n*S;
    % all required numbers to be placed in, without the predefined ones
    % if it's a simple 1-12 number star without predefined numbers, just use: v = uint8(1:12)
    v = uint8([9,6,10,5,8,4,12,6,8]);
    % calculate all permutations
    P = perms(v);

    c = ones(size(P,1),1,'uint8');
    % if it's a simple 1-12 number star without predefined numbers, just shop the P = [P....] part
    % put the predefined numbers at the required colums (position) and fill with the
    % remaining ones
    % first from "v", the second is the 5, then 2-4 from predefined "v" and
    % a number 4 then 5-7 from "v" then a number 1 and the rest of the
    % array
    P = [P(:,1:1) 5*c P(:,2:4) 4*c P(:,5:7) 1*c P(:,8:9)]; 
    % free up some memory
    clear c;

    % The line sum requirements
    f = find(P(:,1)+P(:,2)+P(:,4)+P(:,5) == L & ...
            P(:,3)+P(:,4)+P(:,6)+P(:,7) == L & ...
            P(:,5)+P(:,6)+P(:,8)+P(:,9) == L & ...
            P(:,7)+P(:,8)+P(:,10)+P(:,11) == L & ...
            P(:,9)+P(:,10)+P(:,12)+P(:,1) == L & ...
            P(:,11)+P(:,12)+P(:,2)+P(:,3) == L)

    P = P(f,:)

    observatory(P(1,:))

end

function observatory(v)
   % observatory  View Magic Stars.
   % observatory(v), where v is a vector of 10 or 12 integers.
      clf
      n = length(v)/2;
      d = pi/n;
      t = 0:2*n-1;
      r0 = .42 + .20*(n == 5);
      r = 1 - r0*(mod(t,2)==1);
      x = r.*sin(t*d);
      y = r.*cos(t*d);
      p = plot(x,y,'o','markersize',28,'markeredgecolor',[0 0 .66]);
      axis(1.25*[-1 1 -1 1])
      axis square
      set(gca,'xtick',[],'ytick',[])
      if n == 5
         h = line([x(1:4:2*n) x(3:4:2*n) x(1)], ...
                  [y(1:4:2*n) y(3:4:2*n) y(1)]);
      elseif n == 6
         h = [line([x(1:4:2*n) x(1)],[y(1:4:2*n) y(1)])
              line([x(3:4:2*n) x(3)],[y(3:4:2*n) y(3)])];
      else
         error('Can only observe 10 or 12 points')
      end
      set(h,'color',[0 .66 .66])
      for k = 1:2*n
         text(x(k),y(k),int2str(v(k)),'fontsize',18,'horiz','center')
      end
end
```
