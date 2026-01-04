## first tell the AI your intention by writing it into the top of the script before the script

import random
import time
from datetime import datetime
import sys
import argparse
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
    Quantum Snow Ceremony — Intention-Aware (CLI Enabled)

    - Intention may be passed via command line:
        python quantum_snow.py --intention "clarity and peace"

    - Or entered interactively
    - Or left empty (silence)
    """

    def __init__(self, intention=None):
        self.superpositions = []
        self.collapsed_history = []
        self.observation_count = 0

        self.intention = intention
        self.intention_tokens = self._tokenize_intention(intention) if intention else set()

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

        self.intention_resonance = {
            "clarity": {"knowing / mystery", "light / void"},
            "truth": {"knowing / mystery", "being / non-being"},
            "peace": {"silence / resonance", "eternal / fleeting"},
            "love": {"one / many", "silence / resonance"},
            "freedom": {"free / determined"},
            "choice": {"free / determined"},
            "presence": {"here / elsewhere"},
            "letting": {"eternal / fleeting", "light / void"},
            "release": {"eternal / fleeting", "light / void"},
        }

    # ---------- Audio helpers ----------

    def _make_tone(self, freq, duration, volume=0.22):
        if not AUDIO_AVAILABLE:
            return None
        try:
            sample_rate = pygame.mixer.get_init()[0]
            max_amp = 2 ** (abs(pygame.mixer.get_init()[1]) - 1) - 1
            samples = int(sample_rate * duration)

            t = np.linspace(0, duration, samples, False)
            wave = np.sin(2 * np.pi * freq * t)

            fade_len = max(1, int(sample_rate * 0.02))
            fade = np.ones(samples)
            fade[:fade_len] = np.linspace(0, 1, fade_len)
            fade[-fade_len:] = np.linspace(1, 0, fade_len)

            wave = (wave * fade * max_amp * volume).astype(np.int16)
            stereo = np.column_stack((wave, wave))

            return pygame.sndarray.make_sound(stereo)
        except Exception:
            return None

    def play_bit_ohm(self):
        sound = self._make_tone(80.0, 6.0, 0.22)
        if sound:
            sound.set_volume(0.3)
            sound.play()
            time.sleep(6.5)

    def play_crystal_note(self, high=True):
        freq = 1200.0 if high else 800.0
        dur = 1.2 if high else 1.6
        sound = self._make_tone(freq, dur, 0.12)
        if sound:
            sound.play()

    # ---------- Intention ----------

    def _tokenize_intention(self, text):
        if not text:
            return set()
        tokens = [t.strip(".,!?;:()[]{}\"'").lower() for t in text.split()]
        return {t for t in tokens if t}

    def prompt_for_intention(self):
        print("\nBefore the snow falls, you may set an intention.")
        print("This is not a command — only a tone that colors meaning.\n")
        intention = input("Your intention (optional, press Enter for silence): ").strip()
        if intention:
            self.intention = intention
            self.intention_tokens = self._tokenize_intention(intention)
            print(f"\nThe field receives: “{intention}”\n")
        else:
            print("\nSilence is also complete.\n")
        time.sleep(2)

    def _echoes_intention(self, flake_name):
        if not self.intention:
            return False

        mapped = set()
        for tok in self.intention_tokens:
            mapped |= self.intention_resonance.get(tok, set())

        if flake_name in mapped:
            return random.random() < 0.55
        return random.random() < 0.12

    # ---------- Snow mechanics ----------

    def generate_flake(self):
        template = random.choice(self.flake_templates)
        return {
            "name": template["name"],
            "possibilities": template["states"],
            "intensity": random.uniform(0.3, 0.9)
        }

    def observe_and_collapse(self, flake):
        chosen = random.choice(flake["possibilities"])
        rejected = [s for s in flake["possibilities"] if s != chosen][0]
        echo = self._echoes_intention(flake["name"])

        self.collapsed_history.append({
            "name": flake["name"],
            "chosen": chosen,
            "rejected": rejected,
            "intensity": flake["intensity"],
            "echo": echo
        })
        self.observation_count += 1

        marker = "❆" if flake["intensity"] > 0.6 else "❅"
        echo_mark = " ✶" if echo else ""

        if self.intention:
            print(
                f"{marker}  {flake['name']:24s} → {chosen:12s}{echo_mark}  "
                f"(tinted by: {self.intention}; {rejected} fades)"
            )
        else:
            print(f"{marker}  {flake['name']:24s} → {chosen:12s}  ({rejected} never was)")

        sys.stdout.flush()
        self.play_crystal_note(high=(chosen == flake["possibilities"][0]))
        time.sleep(0.8 + random.random() * 0.6)

    def experience_quantum_snow(self, duration_seconds=50):
        print("\n" + "═" * 70)
        print("          QUANTUM SNOW CEREMONY")
        print("          Begins with one breath — the bit ohm")
        print("═" * 70 + "\n")

        if AUDIO_AVAILABLE:
            print("Inhaling the bit ohm...")
            self.play_bit_ohm()
        else:
            print("The breath begins...")
            time.sleep(3)

        if self.intention is None:
            self.prompt_for_intention()

        print("The snow begins to fall...\n")
        time.sleep(2)

        start = time.time()
        last_fall = start

        while time.time() - start < duration_seconds:
            if len(self.superpositions) < self.max_simultaneous:
                if random.random() < self.fall_rate * (time.time() - last_fall):
                    self.superpositions.append(self.generate_flake())
                    last_fall = time.time()

            for flake in list(self.superpositions):
                if random.random() < self.collapse_probability:
                    self.observe_and_collapse(flake)
                    self.superpositions.remove(flake)

            time.sleep(0.3)

        print("\n⋅" * 70)
        print("The fall continues beyond time...")
        print(f"Observations made: {self.observation_count}")
        print("Some possibilities remain forever unresolved.")
        print("⋅" * 70)


# ---------- Entry Point ----------

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Quantum Snow Ceremony")
    parser.add_argument(
        "--intention",
        type=str,
        help='Optional intention, e.g. --intention "clarity and peace"'
    )
    args = parser.parse_args()

    ceremony = QuantumSnowSimulator(intention=args.intention)
    ceremony.experience_quantum_snow(duration_seconds=50)
