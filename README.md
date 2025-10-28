# WEEK-2
AI BIGDATA WEEK 2
# Generative Abstract Poster - "Cascade Dreams"
# Concepts: flow fields, organic curves, layered transparency, rhythm

import matplotlib.pyplot as plt
import numpy as np
import random
from matplotlib.patches import Circle, Polygon
from matplotlib.collections import PatchCollection

def flowing_ribbon(start_x, start_y, length=15, segments=80, amplitude=2.0, frequency=3):
    """
    Generate a smooth, flowing ribbon path using sine waves.
    """
    t = np.linspace(0, length, segments)

    # Create flowing path with multiple frequency components
    x = start_x + t * np.cos(np.radians(20))
    y = start_y + t * np.sin(np.radians(20)) + \
        amplitude * np.sin(frequency * t / length * 2 * np.pi) + \
        amplitude * 0.5 * np.sin(frequency * 1.7 * t / length * 2 * np.pi)

    return x, y

def create_ribbon_band(x_path, y_path, width=0.4):
    """
    Convert a path into a ribbon band with width.
    """
    # Calculate perpendicular offsets
    dx = np.diff(x_path)
    dy = np.diff(y_path)

    # Append last difference to match array size
    dx = np.append(dx, dx[-1])
    dy = np.append(dy, dy[-1])

    # Normalize and rotate 90 degrees
    length = np.sqrt(dx**2 + dy**2)
    perp_x = -dy / length * width
    perp_y = dx / length * width

    # Create upper and lower bounds
    x_upper = x_path + perp_x
    y_upper = y_path + perp_y
    x_lower = x_path - perp_x
    y_lower = y_path - perp_y

    # Combine into closed polygon
    x_band = np.concatenate([x_upper, x_lower[::-1]])
    y_band = np.concatenate([y_upper, y_lower[::-1]])

    return x_band, y_band

def ocean_twilight_palette():
    """
    Deep blues, purples, and teals with coral accents.
    """
    return [
        (0.15, 0.25, 0.45, 0.7),  # Deep blue
        (0.25, 0.35, 0.55, 0.65),  # Medium blue
        (0.20, 0.45, 0.55, 0.6),   # Teal
        (0.35, 0.25, 0.50, 0.65),  # Purple
        (0.55, 0.30, 0.45, 0.6),   # Mauve
        (0.85, 0.45, 0.40, 0.5),   # Coral accent
    ]

def add_organic_circles(ax, n_circles=25, x_range=(-2, 12), y_range=(-2, 16)):
    """
    Add scattered organic circles of varying sizes.
    """
    colors = ocean_twilight_palette()

    for _ in range(n_circles):
        x = random.uniform(*x_range)
        y = random.uniform(*y_range)
        r = random.uniform(0.1, 0.6)
        color = random.choice(colors)

        circle = Circle((x, y), r,
                       color=color,
                       alpha=random.uniform(0.3, 0.6),
                       zorder=random.randint(1, 3))
        ax.add_patch(circle)

def generate_cascade_dreams(seed=None, n_ribbons=12):
    """
    Generate a flowing, cascading abstract poster.
    """
    if seed is not None:
        np.random.seed(seed)
        random.seed(seed)

    fig, ax = plt.subplots(figsize=(6, 8))
    ax.set_aspect('equal')
    ax.axis('off')
    ax.set_facecolor('#f8f5f0')  # Warm off-white

    colors = ocean_twilight_palette()

    # Generate flowing ribbons at different positions
    for i in range(n_ribbons):
        # Stagger starting positions
        start_x = -2 + (i % 3) * 2 + random.uniform(-0.5, 0.5)
        start_y = 14 - i * 1.3 + random.uniform(-0.3, 0.3)

        # Vary parameters for each ribbon
        amplitude = random.uniform(1.5, 3.0)
        frequency = random.uniform(2, 4)
        width = random.uniform(0.25, 0.5)

        x_path, y_path = flowing_ribbon(start_x, start_y,
                                        length=random.uniform(12, 18),
                                        amplitude=amplitude,
                                        frequency=frequency)

        x_band, y_band = create_ribbon_band(x_path, y_path, width=width)

        color = colors[i % len(colors)]
        ax.fill(x_band, y_band, color=color, alpha=0.65, zorder=2)

    # Add organic circles for texture
    add_organic_circles(ax, n_circles=30)

    # Add a few accent ribbons with different flow
    for _ in range(3):
        start_x = random.uniform(2, 8)
        start_y = random.uniform(2, 12)
        x_path, y_path = flowing_ribbon(start_x, start_y,
                                        length=8,
                                        amplitude=1.5,
                                        frequency=5)
        x_band, y_band = create_ribbon_band(x_path, y_path, width=0.2)
        ax.fill(x_band, y_band, color=colors[-1], alpha=0.4, zorder=3)

    # Layout
    ax.set_xlim(-2, 12)
    ax.set_ylim(-2, 16)

    # Text labels
    ax.text(0.5, 15.2, "Week2 • Arts & Advanced Big Data",
            fontsize=13, weight='bold', ha="left", va="top",
            color='#2a3a4a')

    ax.text(0.5, 14.5, "Generative Poster – Cascade Dreams",
            fontsize=10, style='italic', ha="left", va="top",
            color='#3a4a5a')

    return fig


# --- Run ---
fig = generate_cascade_dreams(seed=42, n_ribbons=14)
fig.savefig("poster_cascade_dreams.png", dpi=300, bbox_inches="tight")
plt.show()
