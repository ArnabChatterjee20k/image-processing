# Square Crop Centered on Expanded Bounding Box

## Example Scenario
- **Original Image Size:** 2000x2000 pixels  
- **Expanded Bounding Box (after `expand_ratio` and `head_offset`):**  
  - Top-left corner: `(x=380, y=225)`  
  - Width: `w=640`  
  - Height: `h=1400`  
  
## Goal
Create a square crop centered on the expanded bounding box.

---

## Step-by-Step Breakdown

### Step 1: Expand the Bounding Box Width
#### Math:
Expand width by 30% on both sides:
```python
x_new = 500 - (400 * 0.3) = 500 - 120 = 380
w_new = 400 + (400 * 0.3 * 2) = 400 + 240 = 640
```
New width bounds: `x=380` to `x+640=1020`.

**Reason:**
Adds context around the face (e.g., hair, shoulders) to avoid tight cropping.

---

### Step 2: Adjust the Bounding Box Height for Head Centering
#### Math:
Shift the bounding box upward by 75% of the original height:
```python
y_new = 600 - (500 * 0.75) = 600 - 375 = 225
```
Expand height to include the head and body:
```python
h_new = 500 + (500 * (0.3 + 0.75*2)) = 500 + (500 * 1.8) = 1400
```
New height bounds: `y=225` to `y+1400=1625`.

**Reason:**
Ensures the top of the head (e.g., forehead, hair) is included in the crop.

---

### Step 3: Find the Center of the Expanded Bounding Box
#### Center X-coordinate:
\[ x_{center} = x + \frac{w}{2} = 380 + \frac{640}{2} = 380 + 320 = 700 \]

#### Center Y-coordinate:
\[ y_{center} = y + \frac{h}{2} = 225 + \frac{1400}{2} = 225 + 700 = 925 \]

The center `(700, 925)` becomes the anchor point for the square crop.

---

### Step 4: Determine the Size of the Square
The square’s side length is the largest dimension of the expanded bounding box:
\[ crop\_size = \max(w, h) = \max(640, 1400) = 1400 \]

We need a square large enough to fully contain the expanded bounding box (including the head and shoulders).

---

### Step 5: Calculate the Square’s Boundaries
#### Top-Left Corner of the Square:
\[ x_{start} = x_{center} - \frac{crop\_size}{2} = 700 - \frac{1400}{2} = 700 - 700 = 0 \]
\[ y_{start} = y_{center} - \frac{crop\_size}{2} = 925 - \frac{1400}{2} = 925 - 700 = 225 \]

#### Bottom-Right Corner of the Square:
\[ x_{end} = x_{start} + crop\_size = 0 + 1400 = 1400 \]
\[ y_{end} = y_{start} + crop\_size = 225 + 1400 = 1625 \]

#### Final Square Coordinates:
From `(0, 225)` to `(1400, 1625)` (a `1400x1400` square).

---

### Step 6: Visualization
- The expanded bounding box (`640x1400`) is centered inside the square (`1400x1400`).
- The square’s center `(700, 925)` matches the expanded box’s center.
- The square extends equally in all directions from this center point.

This ensures the expanded bounding box is completely contained within the final crop.

