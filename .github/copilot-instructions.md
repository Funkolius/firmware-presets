# Copilot Instructions for firmware-presets

## Project Overview
This repository provides Betaflight firmware presets for various versions and hardware configurations. Presets are organized by firmware version and category (e.g., filters, rates, osd, etc.).

## Key Architecture
- Presets are stored as plain text files under `presets/<version>/<category>/`.
- Each preset file contains Betaflight CLI commands.
- The main categories are: `filters`, `rates`, `osd`, `other`, etc.
- The `indexer/` directory contains scripts for indexing and managing preset files.

## Preset Application Logic
- **PID values** are always applied when a preset is selected in Betaflight. No options are needed for PIDs.
- **Filters** are the only category with selectable options. These options allow users to choose different filter settings (e.g., Dshot300, Dshot600).

## Filter Option Convention
- Filter options are separated using explicit markers:
  - `# OPTION BEGIN (UNCHECKED): <OptionName>` starts an option block
  - `# OPTION END` ends the block
- Example:
  ```
  # OPTION BEGIN (UNCHECKED): Dshot300
      set motor_pwm_protocol = Dshot300
  # OPTION END

  # OPTION BEGIN (UNCHECKED): Dshot600
      set motor_pwm_protocol = DSHOT600
  # OPTION END
  ```
- Only filter-related settings use these option blocks. All other preset categories are applied directly without options.

## Developer Workflow
- To add a new preset, create a new `.txt` file in the appropriate version/category folder.
- For filter presets, use the option block convention above.
- Run scripts in `indexer/` to update or validate the preset index as needed.
- No build or test system is present; changes are managed by file edits and indexing scripts.

## Integration Points
- Presets are consumed by Betaflight via CLI import or through the Betaflight Configurator.
- No external dependencies or complex build steps.

## Example Files
- `presets/4.5/filters/karate_array.txt` — demonstrates filter option blocks
- `indexer/indexer.js` — main indexing logic

## Summary
- **Always apply PIDs by default.**
- **Only filters have selectable options, using the defined block markers.**
- **Keep preset files simple and readable.**
- **Use the `indexer/` scripts for management.**

## RPM Filter Warning
 If a preset uses RPM filtering, it is ONLY safe with DSHOT 600 or DSHOT 300. Using any other motor protocol is extremely unsafe.
 See `misc/warnings/en/rpm_filters.txt` for the official warning text.

For questions or improvements, follow the conventions above for consistency and maintainability.
