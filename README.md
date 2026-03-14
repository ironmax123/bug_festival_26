7# bug_festival_26

```
import math
import csv
import matplotlib.pyplot as plt

# =========================
# 設定
# =========================
RADIUS = 250            # 半径
SPACING = 5             # 円周方向の間隔（ブロック）
CENTER_X = 0            # 中心X
CENTER_Z = 0            # 中心Z
OUTPUT_CSV = "circle_pillars_r250.csv"


# =========================
# 4分の1だけ作る
# =========================
def generate_quarter_points(radius, spacing):
    """
    第1象限（x>=0, z>=0）のみ生成
    円周方向に spacing 間隔になるよう角度を刻む
    """
    dtheta = spacing / radius   # 弧長 s = rθ より θ = s/r
    points = []

    theta = 0.0
    while theta <= math.pi / 2 + 1e-9:
        x = round(radius * math.cos(theta))
        z = round(radius * math.sin(theta))
        points.append((x, z))
        theta += dtheta

    # 重複除去しつつ順序保持
    unique = []
    seen = set()
    for p in points:
        if p not in seen:
            seen.add(p)
            unique.append(p)

    return unique


# =========================
# 4分の1から全周へ展開
# =========================
def expand_full_circle(quarter_points):
    """
    第1象限の点から4象限へ対称展開
    """
    full = set()

    for x, z in quarter_points:
        candidates = [
            ( x,  z),
            (-x,  z),
            ( x, -z),
            (-x, -z),
        ]
        for p in candidates:
            full.add(p)

    # 角度順に並べる
    full = sorted(full, key=lambda p: math.atan2(p[1], p[0]))
    return full


# =========================
# 中心座標を加算
# =========================
def shift_points(points, cx, cz):
    return [(cx + x, cz + z) for x, z in points]


# =========================
# CSV保存
# =========================
def save_csv(points, filename):
    with open(filename, "w", newline="", encoding="utf-8") as f:
        writer = csv.writer(f)
        writer.writerow(["No", "X", "Z"])
        for i, (x, z) in enumerate(points, start=1):
            writer.writerow([i, x, z])


# =========================
# プロット
# =========================
def plot_blueprint(full_points, quarter_points, radius, cx, cz):
    fig, ax = plt.subplots(figsize=(10, 10))

    # 全周の柱
    xs = [p[0] for p in full_points]
    zs = [p[1] for p in full_points]
    ax.scatter(xs, zs, s=18, label="Pillars (full circle)")

    # 第1象限だけ色を変えて確認用に重ねる
    qxs = [cx + p[0] for p in quarter_points]
    qzs = [cz + p[1] for p in quarter_points]
    ax.scatter(qxs, qzs, s=30, marker="x", label="Quarter used for symmetry")

    # 中心
    ax.scatter([cx], [cz], s=50, marker="o", label="Center")

    # 参考用の真円
    circle_x = []
    circle_z = []
    for i in range(721):
        t = 2 * math.pi * i / 720
        circle_x.append(cx + radius * math.cos(t))
        circle_z.append(cz + radius * math.sin(t))
    ax.plot(circle_x, circle_z, linewidth=1)

    # 見た目調整
    ax.set_title(f"Minecraft Circle Pillar Blueprint (radius={radius}, spacing≈{SPACING})")
    ax.set_xlabel("X")
    ax.set_ylabel("Z")
    ax.set_aspect("equal")
    ax.grid(True)
    ax.legend()
    plt.tight_layout()
    plt.show()


# =========================
# メイン
# =========================
def main():
    quarter = generate_quarter_points(RADIUS, SPACING)
    full_local = expand_full_circle(quarter)
    full_world = shift_points(full_local, CENTER_X, CENTER_Z)

    save_csv(full_world, OUTPUT_CSV)

    circumference = 2 * math.pi * RADIUS
    expected_count = circumference / SPACING

    print(f"半径: {RADIUS}")
    print(f"設定間隔: {SPACING}")
    print(f"円周: {circumference:.2f}")
    print(f"理論上の点数目安: {expected_count:.2f}")
    print(f"実際の柱数: {len(full_world)}")
    print(f"CSV保存: {OUTPUT_CSV}")
    print()
    print("先頭20件:")
    for i, p in enumerate(full_world[:20], start=1):
        print(f"{i:3d}: X={p[0]:4d}, Z={p[1]:4d}")

    plot_blueprint(full_world, quarter, RADIUS, CENTER_X, CENTER_Z)


if __name__ == "__main__":
    main()
```
const r = 250;
const s = 5;

function buildBeam(x, z) {
  // ここに梁を建設するコードを書く
  // 例:
  // setBlock(x, y, z, "minecraft:stone");
}

for (let n = 0; n < (2 * Math.PI * r) / s; n++) {
  const theta = (n * s) / r;

  const x = Math.round(r * Math.cos(theta));
  const z = Math.round(r * Math.sin(theta));

  buildBeam(x, z);
}
```
const r = 250;
const s = 5;

function buildBeam(x, z) {
  // ここに梁を建設するコードを書く
  // 例:
  // setBlock(x, y, z, "minecraft:stone");
}

for (let n = 0; n < (2 * Math.PI * r) / s; n++) {
  const theta = (n * s) / r;

  const x = Math.round(r * Math.cos(theta));
  const z = Math.round(r * Math.sin(theta));

  buildBeam(x, z);
}

```


```
function buildBeam(x: number, z: number) {
    blocks.place(STONE, positions.add(start, pos(x, 0, z)))
}
```