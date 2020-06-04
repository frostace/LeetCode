# **. **

[LeetCode ]()

## Problem

Quad Tree

## Methods
Intuition: 


### Code
```JavaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.myQuad = null;
    }
}

class Area {
    constructor(x, y, w, h) {
        this.x = x;
        this.y = y;
        this.w = w;
        this.h = h;
    }

    contains(point) {
        return (
            Math.abs(point.x - this.x) < this.w / 2 &&
            Math.abs(point.y - this.y) < this.h / 2
        );
    }

    intersects(area) {
        return (
            area.x - area.w / 2 > this.x + this.w / 2 &&
            this.x - this.w / 2 > area.x + area.w / 2 &&
            area.y - area.h / 2 > this.y + this.h / 2 &&
            this.y - this.h / 2 > area.y + area.h / 2
        );
    }
}

class QuadTree {
    constructor(initArea, n) {
        this.area = initArea;
        this.capacity = n;
        this.points = [];
        this.divided = false;
      	this.personalColor = [
            Math.floor(Math.random() * 255),
            Math.floor(Math.random() * 255),
            Math.floor(Math.random() * 255),
        ];
    }

    redistribute() {
        for (let point of this.points) {
            this.topLeft.insert(point);
            this.topRight.insert(point);
            this.bottomLeft.insert(point);
            this.bottomRight.insert(point);
        }
    }

    subdivide() {
        let x = this.area.x,
            y = this.area.y,
            w = this.area.w,
            h = this.area.h;
        let topLeftArea = new Area(x - w / 4, y - h / 4, w / 2, h / 2);
        let topRightArea = new Area(x + w / 4, y - h / 4, w / 2, h / 2);
        let bottomLeftArea = new Area(x - w / 4, y + h / 4, w / 2, h / 2);
        let bottomRightArea = new Area(x + w / 4, y + h / 4, w / 2, h / 2);
        this.topLeft = new QuadTree(topLeftArea, this.capacity);
        this.topRight = new QuadTree(topRightArea, this.capacity);
        this.bottomLeft = new QuadTree(bottomLeftArea, this.capacity);
        this.bottomRight = new QuadTree(bottomRightArea, this.capacity);
        this.divided = true;
        this.redistribute();
        this.points = [];
    }

    insert(point) {
        // edge case
        if (!this.area.contains(point)) return false;
        if (this.points.length < this.capacity && !this.divided) {
            this.points.push(point);
            // (re-)assign Tree to this snowflake
            point.resideTree = this;
            return true;
        } else {
            this.divided || this.subdivide();
         		// once we inserted point to a correct sub quad tree, we stop
            this.topLeft.insert(point) ||
                this.topRight.insert(point) ||
                this.bottomLeft.insert(point) ||
                this.bottomRight.insert(point);
        }

        //TODO: if all sub quad trees are found to be empty, merge all sub trees?
    }

    query(targetArea, nearbyPoints) {
        if (this.area.intersects(targetArea)) return;

        // if not divided, insert snowflakes
        for (let point of this.points) {
            !this.divided &&
                targetArea.contains(point) &&
                nearbySnowflakes.push(point);
        }

        // if it's divided, check its children
        if (!this.divided) return;
        this.topLeft.query(targetArea, nearbyPoints);
        this.topRight.query(targetArea, nearbyPoints);
        this.bottomLeft.query(targetArea, nearbyPoints);
        this.bottomRight.query(targetArea, nearbyPoints);
    }

    // help visualize quad tree area and snowflake nums in this tree
  	// used for debug
    show() {
        stroke(
            this.personalColor[0],
            this.personalColor[1],
            this.personalColor[2]
        );
        strokeWeight(1);
        noFill();
        rectMode(CENTER);
        rect(this.area.x, this.area.y, this.area.w, this.area.h);

        textSize(14);
        fill(255);
        text(
            this.points.length.toString(),
            this.area.x - this.area.w / 2 + 10,
            this.area.y - this.area.h / 2 + 15
        );

        if (this.divided) {
            this.topLeft.show();
            this.topRight.show();
            this.bottomLeft.show();
            this.bottomRight.show();
        }
        for (let p of this.points) {
            let c;
            if (p.outOfCurrArea()) {
                c = color(255, 204, 0);
                // console.log(p.pos.x, p.pos.y, p.resideTree);
            } else
                c = color(
                    this.personalColor[0],
                    this.personalColor[1],
                    this.personalColor[2]
                );

            stroke(c);
            strokeWeight(1);
            noFill();
            rectMode(CENTER);
            rect(p.pos.x, p.pos.y, 10, 10);
        }
    }
}

```

### Reference

