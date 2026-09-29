# QR codes

Codes for the site itself, for slides, print and conference handouts. **No
page on the site uses these** — a QR code is for getting someone from the
physical world to the site, and a reader who is already on the site does not
need one. They live in the repo so there is one canonical set and nobody
regenerates a slightly different one the week of an event.

| File | What it is | Reach for it when |
|---|---|---|
| `wip-qr.svg` | Vector master. | Print, or anything larger than a slide. Scales to any size without softening. |
| `wip-qr-plum.png` | 1960 × 1960, plum on white. | Slides and digital handouts, on a light ground. |
| `wip-qr-black.png` | 1960 × 1960, black on white. | Anything printed in one color, or where plum would fight the artwork. |

All three encode the same URL:

```
https://womensinnovationnetwork.github.io/wip/
```

## Rules

1. **Scan it after you place it.** Not the file, the finished artifact: the
   exported slide, the printed proof. Contrast, scale and compression all
   break codes, and a code nobody checked is worse than no code at all.
2. **Keep the quiet zone.** The white margin built into these files is part
   of the code. Cropping to the edge of the squares stops it scanning.
3. **Never re-color the squares** to something lighter than the plum here,
   and never put the code on a photo or a gradient. Dark on light, always.
4. **Minimum size**: about 25mm printed, or 120px on screen. Below that,
   phone cameras start to struggle in bad light, which is most conference
   halls.
5. **Print the URL next to it too.** Some people will not scan a code, and
   some scanners are broken. The address is short enough to type.

## Regenerating

Only when the destination URL changes. If it does, replace all three and
re-verify each one decodes before committing.

```python
import qrcode
qr = qrcode.QRCode(error_correction=qrcode.constants.ERROR_CORRECT_M, border=4)
qr.add_data("https://womensinnovationnetwork.github.io/wip/")
qr.make(fit=True)
qr.make_image(fill_color="#6a2db4", back_color="white").save("wip-qr-plum.png")
```

Error correction level **M** is the right trade-off here: it survives a
scuffed print without making the code denser than it needs to be. Do not add
a logo in the middle — that eats the error-correction budget the scuffing
needs.

Verify with:

```python
import cv2, numpy as np
from PIL import Image
im = Image.open("wip-qr-plum.png").convert("RGB")
# Downscale first: the detector often fails on a very large image, which
# looks like a broken code and is not one.
im = im.resize((im.width // 3, im.height // 3), Image.LANCZOS)
print(cv2.QRCodeDetector().detectAndDecode(np.array(im)[:, :, ::-1])[0])
```

## Other codes

The PPCC WhatsApp group code is **not** here. It belongs to one page and one
event, so it sits with that page's images in `assets/img/ppcc/`, and it comes
down when the event does.
