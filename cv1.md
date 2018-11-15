```python

BLACK = 0
WHITE = 255


threshold = 100
table = []
for i in range(256):
    if i < threshold:
        table.append(0)
    else:
        table.append(1)

if __name__ == '__main__':
    origin_pic = Image.open(origin)
    # convert the original image to a gray image
    gray = origin_pic.convert('L')
    # convert a gray image to a binary image
    bin = gray.point(table, '1')
    bin.save(p_bin)
```

```python
def is_intersect(kernel, img_slice):
    for i, row in enumerate(img_slice):
        for j, value in enumerate(row):
            if value == BLACK and kernel[i, j] == 1:
                return True
    return False

def dilation_bin(img, kernel, path):
    w, h = kernel.shape
    row = (w-1)//2
    col = (h-1)//2
    img_pad = np.pad(img, ((row, row), (col, col)), 'edge')
    res = np.zeros(img.shape)
    for r, row in enumerate(img):
        for c, col in enumerate(row):
            if is_intersect(kernel, img_pad[r:r+w, c:c+h]):
                res[r, c] = BLACK
            else:
                res[r, c] = WHITE
```

```python
def is_cover(kernel, img_slice):
    for i, row in enumerate(img_slice):
        for j, value in enumerate(row):
            if value == BLACK and kernel[i, j] == 1:
                continue
            if value == WHITE and kernel[i, j] == 1:
                return False
    return True


def erosion_bin(img, kernel, path):
    w, h = kernel.shape
    row = (w-1)//2
    col = (h-1)//2
    img_pad = np.pad(img, ((row, row), (col, col)), 'edge')
    res = np.zeros(img.shape)
    for r, row in enumerate(img):
        for c, col in enumerate(row):
            if is_cover(kernel, img_pad[r:r+w, c:c+h]):
                res[r, c] = BLACK
            else:
                res[r, c] = WHITE
```

```python
kernel = np.array([[1, 1, 1],
                   [1, 1, 1],
                   [1, 1, 1]])

kernel1 = np.array([[0, 1, 0],
                    [1, 1, 1],
                    [0, 1, 0]])

kernel2 = np.array([[0, 0, 1, 0, 0],
                    [0, 1, 1, 1, 0],
                    [1, 1, 1, 1, 1],
                    [0, 1, 1, 1, 0],
                    [0, 0, 1, 0, 0]])

kernel3 = np.ones((5, 5))
```

