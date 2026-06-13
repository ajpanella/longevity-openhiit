# Longevity Lifestyle Timer Adaptation Plan

This fork exists as a technical starting point for the Longevity Lifestyle Exercise tab. The goal is not to ship OpenHIIT as-is. The goal is to extract and adapt the strongest timer behaviors into a branded, client-facing longevity app with a Seconds-style custom circuit builder.

## Product Direction

- Build toward a Seconds-style exercise experience: fast circuit creation, reusable templates, clear interval flow, strong visual cues, sound or voice cues, and simple editing.
- Keep the Longevity app philosophy: Simple Systems, Incremental Goals, N of 1.
- Make the Exercise tab feel like part of the broader client hub, not a standalone workout timer app.
- Treat OpenHIIT as the timer and interval reference implementation, especially for interval generation, background timing, pause/resume, visual/audio cues, and saved timer definitions.

## What To Keep From OpenHIIT

- `background_hiit_timer` integration through `Countdown`, `CountdownController`, `TimerState`, and `IntervalType`.
- Timer lifecycle behavior from `lib/features/run_timer/workout.dart`.
- Interval generation concepts from `lib/core/utils/interval_calculation.dart`.
- Timer persistence concepts from `TimerType`, `TimerTimeSettings`, `TimerSoundSettings`, and the repository layer.
- Import/export concepts for future coach-created circuit sharing.
- Integration test ideas around custom timers, interval adjustment, restarts, and edit flows.

## What To Replace

- Replace the OpenHIIT home and timer-list UX with the Longevity bottom-tab shell.
- Replace OpenHIIT visual styling with the Longevity palette:
  - Primary: `#0A3D2B`
  - Primary dark: `#061F17`
  - Primary soft: `#DDF7EA`
  - Accent: `#4ADE80`
  - Milestone gold: `#FACC15`
- Replace generic timer labels with program-aware language:
  - HIIT
  - Circuit
  - Tabata
  - Yoga / stretching flow
  - Recovery movement
- Replace standalone timer storage with the future app-wide client data model.

## Suggested Flutter Module Shape

```text
lib/
  features/
    exercise/
      data/
        exercise_timer_repository.dart
        exercise_timer_models.dart
      domain/
        interval_sequence_builder.dart
        workout_completion_event.dart
      ui/
        exercise_tab.dart
        circuit_builder_screen.dart
        run_exercise_timer_screen.dart
        widgets/
          timer_ring.dart
          interval_timeline.dart
          circuit_template_card.dart
          rep_logger.dart
```

## Data Model Sketch

```dart
class ExerciseTimerTemplate {
  final String id;
  final String title;
  final ExerciseTimerMode mode; // hiit, tabata, circuit, yogaFlow
  final int workSeconds;
  final int restSeconds;
  final int rounds;
  final int warmupSeconds;
  final int cooldownSeconds;
  final List<ExerciseBlock> blocks;
  final bool voiceCuesEnabled;
  final int color;
}

class ExerciseBlock {
  final String id;
  final String name;
  final int durationSeconds;
  final ExerciseIntervalKind kind; // work, rest, warmup, cooldown, break
  final String? coachCue;
}

class WorkoutCompletionEvent {
  final String templateId;
  final DateTime completedAt;
  final int elapsedSeconds;
  final int completedIntervals;
}
```

## First Implementation Milestones

1. Install Flutter locally and confirm this fork runs with `flutter doctor`, `flutter pub get`, and `flutter run`.
2. Build a small Longevity-branded timer screen around OpenHIIT's `Countdown` integration.
3. Add seeded templates for HIIT, Circuit, Tabata, and Yoga Flow.
4. Map completed workouts into the app-wide Tracker as auto-logged activity.
5. Add a circuit builder that supports named exercises, rounds, rest, break, warmup, cooldown, colors, and voice/audio cue settings.
6. Add coach-created/preloaded program support.

## Known Follow-Ups

- Decide whether to keep `sqflite_common_ffi` for local persistence or move timer templates into the same local/cloud sync layer as the rest of the app.
- Decide whether to fork `a-mabe/background_hiit_timer` for deeper background execution control.
- Add accessibility review for timer color states so cues are not color-only.
- Confirm App Store / Play Store background timer behavior on real devices.
- Preserve the MIT license notice from OpenHIIT in any adapted source distribution.
