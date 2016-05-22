---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-08-18 15:07:19
categories: [tutorial]
comments: true
---

<!--more-->

{% highlight java %}
import javax.swing.*;
import java.awt.*;
public class Square {
    private int x,y;
    private String name;
    public Square(int x, int y)
    {
        this.x = x;
        this.y = y;
        name = "x";
    }
    public Square(int x, int y,String name)
    {
        this.x = x;
        this.y = y;
        this.name = name;
    }
    public int getX()
    {
        return x;
    }
    public int getY()
    {
        return y;
    }
    public String getName()
    {
        return name;
    }
    public void setName(String n)
    {
        name = n;
    }
    public static void draw(Graphics g)
    {
        Square[][] squares = new Square[10][10];
        int x = 100;
        int y = 100;
        for(int r = 0; r < squares.length; r++)
        {
            for(int c = 0; c < squares[0].length; c++)
            {
                  squares[r][c] = new Square(x,y);
                  x += 50;
            }
            y += 50;
            x = 100;
        }
        for(Square[] z : squares)
        {
            for(Square v : z)
            {
                g.setColor(Color.WHITE);
                g.drawRect(v.getX(),v.getY(),50,50);
            }
        }
        Square[][] squares2 = new Square[10][10];
        x = 700;
        y = 100;
        for(int r = 0; r < squares2.length; r++)
        {
            for(int c = 0; c < squares2[0].length; c++)
            {
                squares2[r][c] = new Square(x,y);
                x += 50;
            }
            y += 50;
            x = 700;
        }
        for(Square[] z : squares2)
        {
            for(Square v : z)
            {
                g.setColor(Color.WHITE);
                g.drawRect(v.getX(), v.getY(), 50, 50);
            }
        }
        x = 120;
        for(int i = 1; i != 11; i++)
        {
            g.drawString(i+"",x,90);
            x += 50;
        }
        x = 720;
        for(int i = 1; i != 11; i++)
        {
            g.drawString(i+"",x,90);
            x += 50;
        }
        y = 125;
        char a = 65;
        for(int i = 1; i != 11; i++)
        {
            g.drawString(a+"",80,y);
            y+=50;
            a++;
        }
        a = 65;
        y = 125;
        for(int i = 1; i != 11; i++) {
            g.drawString(a + "", 680, y);
            y += 50;
            a++;
        }
    }
}
{% endhighlight %}
