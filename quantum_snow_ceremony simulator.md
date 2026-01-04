import random
import time
from datetime import datetime
import sys

# Optional audio — very minimal, crystalline tones
try:
    import pygame
    pygame.mixer.init(frequency=44100, size=-16, channels=2, buffer=512)
    AUDIO_AVAILABLE = True
    print("Audio initialized — crystalline tones will accompany the fall.")
except:
    AUDIO_AVAILABLE = False
    print("Running silent — the snow falls without sound.")

class QuantumSnowSimulator:
    """
    Quantum Snow Ceremony
    A silent descent of superpositions.
    No decay. Only collapse upon observation.
    Safe, profound, and strangely peaceful.
    """

    def __init__(self):
        # Internal state: a set of unresolved possibilities
        self.superpositions = []      # List of active quantum flakes
        self.collapsed_history = []   # What actually happened
        self.observation_count = 0
        
        # Snow parameters
        self.fall_rate = 0.4          # Flakes appearing per second (slow, deliberate)
        self.max_simultaneous = 30    # Never too crowded
        self.collapse_probability = 0.12  # Chance a flake collapses this cycle
        
        # Flake types — each carries contradictory truths
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

    def play_crystal_note(self, high=True):
        if not AUDIO_AVAILABLE: return
        try:
            freq = 1200 if high else 800
            duration = 1.8 if high else 2.4
            sample_rate = pygame.mixer.get_init()[0]
            max_amp = 2 ** (abs(pygame.mixer.get_init()[1]) - 1) - 1
            samples = int(sample_rate * duration)
            # Gentle sine-like (square for purity)
            wave = [int(max_amp * 0.18 * ((i // (sample_rate // freq)) % 2 == 0)) for i in range(samples)]
            sound = pygame.sndarray.make_sound(np.array([wave, wave]).T.astype(np.int16))
            sound.play()
        except:
            pass

    def generate_flake(self):
        template = random.choice(self.flake_templates)
        state_a, state_b = template["states"]
        return {
            "name": template["name"],
            "possibilities": [state_a, state_b],
            "born": datetime.now(),
            "intensity": random.uniform(0.3, 0.9)  # Emotional weight
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
        
        # Display
        marker = "❅" if flake["intensity"] < 0.6 else "❆"
        print(f"{marker}  {flake['name']:24s}  →  {chosen:12s}  ({rejected} never was)")
        sys.stdout.flush()
        
        # Crystalline sound on collapse
        self.play_crystal_note(high=(chosen == flake["possibilities"][0]))

    def display_drifting(self, count):
        if count == 0:
            print(" " * 20 + "⋅ ⋅ ⋅ absolute silence ⋅ ⋅ ⋅")
        else:
            snow_line = "❄" * count
            print(f"                         {snow_line}  ({count} unresolved)")
        sys.stdout.flush()

    def experience_quantum_snow(self, duration_seconds=45):
        print("\n" + "═" * 70)
        print("          QUANTUM SNOW CEREMONY")
        print("          A silent fall of superpositions")
        print("          No warmth. No end. Only collapse.")
        print("═" * 70 + "\n")
        
        if AUDIO_AVAILABLE:
            self.play_crystal_note(high=True)
            time.sleep(2)
        
        start_time = time.time()
        last_fall = start_time
        
        print("The snow begins to fall...\n")
        time.sleep(3)
        
        while time.time() - start_time < duration_seconds:
            # Add new flakes slowly
            if len(self.superpositions) < self.max_simultaneous:
                if random.random() < self.fall_rate * (time.time() - last_fall):
                    self.superpositions.append(self.generate_flake())
                    last_fall = time.time()
            
            # Possible collapses
            to_remove = []
            for flake in self.superpositions:
                if random.random() < self.collapse_probability:
                    self.observe_and_collapse(flake)
                    to_remove.append(flake)
                    time.sleep(0.8 + random.random() * 0.6)  # Breathe between collapses
            
            # Remove collapsed
            for r in to_remove:
                if r in self.superpositions:
                    self.superpositions.remove(r)
            
            # Display current drifting superpositions
            if random.random() < 0.3:  # Not every loop — keeps it calm
                self.display_drifting(len(self.superpositions))
            
            time.sleep(0.3)
        
        # Final lingering
        print("\n" + "⋅" * 70)
        print("The fall continues beyond time...")
        print(f"Observations made: {self.observation_count}")
        print("Some possibilities remain forever unresolved.")
        print("⋅" * 70)
        
        if AUDIO_AVAILABLE:
            self.play_crystal_note(high=False)
            time.sleep(3)
        
        self.print_ceremony_summary()
    
    def print_ceremony_summary(self):
        print("\n" + "▲" * 70)
        print("QUANTUM SNOW CEREMONY COMPLETE")
        print("▲" * 70)
        
        print(f"Total observations: {self.observation_count}")
        print(f"Final unresolved superpositions: {len(self.superpositions)}")
        
        if self.collapsed_history:
            print("\nMost Weighted Collapses:")
            sorted_collapses = sorted(self.collapsed_history, key=lambda x: x["intensity"], reverse=True)[:7]
            for i, c in enumerate(sorted_collapses, 1):
                ts = c["timestamp"].strftime("%H:%M:%S")
                print(f"  {i}. {ts} — {c['name']:24s} → {c['chosen']:12s}  ({c['rejected']} erased)")
        
        print("\nThe rest drift on... unobserved, eternal.")
        print("▲" * 70 + "\n")

if __name__ == "__main__":
    ceremony = QuantumSnowSimulator()
    ceremony.experience_quantum_snow(duration_seconds=50)