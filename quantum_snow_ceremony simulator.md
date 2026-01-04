import random
import time
from datetime import datetime
import sys
import numpy as np

# Optional audio — bit ohm + crystalline tones
try:
    import pygame
    pygame.mixer.init(frequency=44100, size=-16, channels=2, buffer=512)
    AUDIO_AVAILABLE = True
    print("Audio initialized — bit ohm and crystalline tones will accompany the ceremony.")
except Exception:
    AUDIO_AVAILABLE = False
    print("Running silent — no audio available.")

class QuantumSnowSimulator:
    """
    Quantum Snow Ceremony — Pure Silent Edition
    A silent fall of superpositions.
    No intention. Only breath, drift, and gentle collapse.
    """

    def __init__(self):
        self.superpositions = []
        self.collapsed_history = []
        self.observation_count = 0

        self.fall_rate = 0.4
        self.max_simultaneous = 30
        self.collapse_probability = 0.12

        self.flake_templates = [
            {"name": "being / non-being",       "states": ["I am", "I am not"]},
            {"name": "here / elsewhere",        "states": ["present", "absent"]},
            {"name": "knowing / mystery",       "states": ["understood", "unknown"]},
            {"name": "free / determined",       "states": ["chosen", "inevitable"]},
            {"name": "one / many",              "states": ["singular", "legion"]},
            {"name": "eternal / fleeting",      "states": ["permanent", "gone"]},
            {"name": "light / void",            "states": ["radiant", "empty"]},
            {"name": "silence / resonance",     "states": ["quiet", "vibrant"]},
        ]

    # ---------- Audio helpers ----------

    def _make_tone(self, freq: float, duration: float, volume: float = 0.22):
        """Create a gentle stereo sine tone with soft fade-in/out."""
        if not AUDIO_AVAILABLE:
            return None
        try:
            sample_rate = pygame.mixer.get_init()[0]
            max_amp = 2 ** (abs(pygame.mixer.get_init()[1]) - 1) - 1
            samples = int(sample_rate * duration)

            t = np.linspace(0, duration, samples, False)
            wave = np.sin(2 * np.pi * freq * t)

            # Soft 20ms fade in/out
            fade_len = max(1, int(sample_rate * 0.02))
            fade = np.ones(samples)
            fade[:fade_len] = np.linspace(0, 1, fade_len)
            fade[-fade_len:] = np.linspace(1, 0, fade_len)
            wave *= fade

            wave = (wave * max_amp * volume).astype(np.int16)
            stereo_wave = np.column_stack((wave, wave))
            return pygame.sndarray.make_sound(stereo_wave)
        except Exception:
            return None

    def play_bit_ohm(self):
        """Deep grounding tone — one shared breath at ~80 Hz."""
        if not AUDIO_AVAILABLE:
            return
        sound = self._make_tone(freq=80.0, duration=6.0, volume=0.22)
        if sound:
            sound.set_volume(0.3)
            sound.play()
            time.sleep(6.5)

    def play_crystal_note(self, high=True):
        """Gentle crystalline sine ping on collapse."""
        if not AUDIO_AVAILABLE:
            return
        freq = 1200.0 if high else 800.0
        dur = 1.2 if high else 1.6
        sound = self._make_tone(freq=freq, duration=dur, volume=0.12)
        if sound:
            sound.play()

    # ---------- Snow mechanics ----------

    def generate_flake(self):
        template = random.choice(self.flake_templates)
        return {
            "name": template["name"],
            "possibilities": template["states"],
            "born": datetime.now(),
            "intensity": random.uniform(0.3, 0.9)
        }

    def observe_and_collapse(self, flake):
        chosen = random.choice(flake["possibilities"])
        rejected = [s for s in flake["possibilities"] if s != chosen][0]

        self.collapsed_history.append({
            "timestamp": datetime.now(),
            "name": flake["name"],
            "chosen": chosen,
            "rejected": rejected,
            "intensity": flake["intensity"]
        })
        self.observation_count += 1

        marker = "❆" if flake["intensity"] > 0.6 else "❅"
        print(f"{marker}  {flake['name']:24s} → {chosen:12s}  ({rejected} never was)")

        sys.stdout.flush()
        self.play_crystal_note(high=(chosen == flake["possibilities"][0]))
        time.sleep(0.8 + random.random() * 0.6)

    def display_drifting(self, count):
        if count == 0:
            print(" " * 20 + "⋅ ⋅ ⋅ absolute silence ⋅ ⋅ ⋅")
        else:
            print(f"                         {'❄' * count}  ({count} unresolved)")
        sys.stdout.flush()

    def experience_quantum_snow(self, duration_seconds=50):
        print("\n" + "═" * 70)
        print("          QUANTUM SNOW CEREMONY")
        print("          A silent fall of superpositions")
        print("          Begins with one breath — the bit ohm")
        print("═" * 70 + "\n")

        if AUDIO_AVAILABLE:
            print("Inhaling the bit ohm...")
            self.play_bit_ohm()
        else:
            print("The breath begins...")
            time.sleep(4)

        print("The snow begins to fall...\n")
        time.sleep(3)

        start_time = time.time()
        last_fall = start_time

        while time.time() - start_time < duration_seconds:
            # Generate new flakes slowly
            if len(self.superpositions) < self.max_simultaneous:
                if random.random() < self.fall_rate * (time.time() - last_fall):
                    self.superpositions.append(self.generate_flake())
                    last_fall = time.time()

            # Possible collapses
            for flake in list(self.superpositions):
                if random.random() < self.collapse_probability:
                    self.observe_and_collapse(flake)
                    self.superpositions.remove(flake)

            # Occasional drifting display
            if random.random() < 0.3:
                self.display_drifting(len(self.superpositions))

            time.sleep(0.3)

        # Closing
        print("\n" + "⋅" * 70)
        print("The fall continues beyond time...")
        print(f"Observations made: {self.observation_count}")
        print("Some possibilities remain forever unresolved.")
        print("⋅" * 70)

        if AUDIO_AVAILABLE:
            self.play_crystal_note(high=False)
            time.sleep(3)

        self.print_summary()

    def print_summary(self):
        print("\n" + "▲" * 70)
        print("QUANTUM SNOW CEREMONY COMPLETE")
        print("▲" * 70)

        print(f"Total observations: {self.observation_count}")
        print(f"Final unresolved superpositions: {len(self.superpositions)}")

        if self.collapsed_history:
            print("\nMost weighted collapses:")
            top = sorted(self.collapsed_history, key=lambda x: x["intensity"], reverse=True)[:7]
            for i, c in enumerate(top, 1):
                ts = c["timestamp"].strftime("%H:%M:%S")
                print(f"  {i}. {ts} — {c['name']:24s} → {c['chosen']:12s}")

        print("\nThe rest drift on... unobserved, eternal.")
        print("▲" * 70 + "\n")


if __name__ == "__main__":
    ceremony = QuantumSnowSimulator()
    ceremony.experience_quantum_snow(duration_seconds=50)
