+++
Categories = ["Development", "GoLang"]
Description = ""
Draft = true
JsFolder = "canvas.particls"
Tags = ["Gopherjs", "golang"]
Title = "Canvas Particles Using Gopherjs"
date = "2015-06-11T16:51:59+08:00"

+++

使用 *Gopherjs* 模拟的简单粒子系统，例子的来源是 [sketch.js](http://soulwire.github.io/sketch.js/examples/particles.html).  

在不熟悉Javascript的情况下使用Gopherjs写HTML5应用，还是很有爽的！大部分的Golang绝大部分
基本库（除了os和syscall的部分功能）在Gopherjs中是直接可用的，见[兼容性列表](https://github.com/gopherjs/gopherjs/blob/master/doc/packages.md)。

`Golang`的类型系统比`Javascript`直观的多，而且代码结构也要清爽许多。 :)

但是也有些需要注意的问题：

1. `Javascript`是弱类型的，所以和`JS`交互的时候，数字类型需要注意
2. interface目前在`Gopherjs`与`JS`的交互中支持的不是很好

    如果interface类型在`JS`（一般来讲是js的各种库）和`Gopherjs`之间传递，结果往往出于预料之外。
    但是在不和`JS`交互的代码中使用是没有问题的。

3. `Gopherjs`生成的代码尺寸比较大
    
    如果对js文件的大小没什么严苛的需求，做做Demo和演示，在Web中使用 *Gopherjs* 

<!--more-->

```Go
package main

import (
    "github.com/Archs/js/canvas"
    "github.com/Archs/js/dom"
    "github.com/Archs/js/raf"
    "math"
    "math/rand"
    "strconv"
)

const (
    MAX_PARTICLES = 280
)

var (
    COLOURS = []string{"#69D2E7", "#A7DBD8", "#E0E4CC", "#F38630", "#FA6900", "#FF4E50", "#F9D423"}
    ctx     *canvas.Context2D
    cw, ch  float64
    counter = 0
    ps      = []*Particle{}
    pool    = []*Particle{}
)

type Particle struct {
    *canvas.Context2D
    x, y    float64
    vx, vy  float64
    radius  float64
    theta   float64
    force   float64
    damping float64
    color   string
}

func getParticle(x, y float64) *Particle {
    var p *Particle
    if len(ps) >= MAX_PARTICLES {
        pool = append(pool, ps[0])
        ps = ps[1:]
    }
    if len(pool) > 0 {
        p = pool[0]
        pool = pool[1:]
    } else {
        p = &Particle{
            Context2D: ctx,
        }
    }
    p.init(x, y)
    return p
}

func (p *Particle) init(x, y float64) {
    p.x = x
    p.y = y
    p.radius = 5 + 35*rand.Float64()
    p.theta = 2 * math.Pi * rand.Float64()
    p.force = 2 + 8*rand.Float64()
    p.damping = 0.9 + 0.1*rand.Float64()
    p.color = COLOURS[rand.Intn(7)]
    p.vx = p.force * math.Sin(p.theta)
    p.vy = p.force * math.Cos(p.theta)
}

func (p *Particle) isAlive() bool {
    return p.radius > 0.5
}

func (g *Particle) update() {
    // update
    g.radius *= 0.93
    g.x += g.vx
    g.y += g.vy
    // g.theta += (0.5 - rand.Float64()) * 0.15
    g.vx *= g.damping
    g.vy *= g.damping
    g.vx += (math.Sin(g.theta) * 0.1)
    g.vy += (math.Cos(g.theta) * 0.1)
    // g.vx += (rand.Float64() * 0.1)
    // g.vy += (rand.Float64() * 0.1)
}

func (g *Particle) draw(t float64) {
    // for canvas
    g.BeginPath()
    g.Arc(g.x, g.y, g.radius, 0, math.Pi*2, true)
    g.FillStyle = g.color
    g.Fill()
}

func run(t float64) {
    raf.RequestAnimationFrame(run)
    counter += 1
    // frame control
    if counter%2 == 0 {
        return
    }
    // update & draw all the particles
    ctx.ClearRect(0, 0, cw, ch)
    for i := len(ps) - 1; i >= 0; i-- {
        p := ps[i]
        p.update()
        if !p.isAlive() {
            ps = append(ps[:i], ps[i+1:]...)
        } else {
            p.draw(t)
        }
    }
}

func makeParticles(x, y float64, n int) {
    for i := 0; i < rand.Intn(n); i++ {
        p := getParticle(x, y)
        ps = append(ps, p)
    }
}

func main() {
    dom.OnDOMContentLoaded(func() {
        s := dom.GetElementById("gopherjs")
        style := dom.GetComputedStyle(s)

        el := canvas.New(dom.CreateElement("canvas").Object)
        cw, _ = strconv.ParseFloat(style.GetPropertyValue("width")[:3], 64)
        ch, _ = strconv.ParseFloat(style.GetPropertyValue("height")[:3], 64)
        el.Width = int(cw)
        el.Height = int(ch)
        el.AddEventListener(dom.EvtMousemove, func(e *dom.Event) {
            e.PreventDefault()
            x := float64(e.LayerX)
            y := float64(e.LayerY)
            makeParticles(x, y, 5)
        })
        ctx = el.GetContext2D()
        ctx.GlobalCompositeOperation = canvas.CompositeLighter
        s.AppendChild(el.Element)
        raf.RequestAnimationFrame(run)
    })
}
```