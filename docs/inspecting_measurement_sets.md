## Inspecting Measurement Sets with `listobs` and `plotms`

After preparing the working directory and extracting the dataset, the next
step is to inspect the contents of the Measurement Set (MS). CASA provides
built-in tasks for examining both the metadata and the visibility data.

---

## Using `listobs` to Inspect the Measurement Set

The `listobs` task is used to generate a summary of the contents of a
Measurement Set. This includes information such as fields, scans, spectral
windows, antennas, and observing setup.

Before running the task, set the visibility dataset:

```bash
vis = 'example.ms'
````

Then run `listobs`:

```bash
listobs()
```

The output is written to the CASA logger. This step is useful for confirming:

* The dataset loaded correctly
* The number of antennas and baselines
* The available fields and spectral windows
* That the Measurement Set is not corrupted

Running `listobs` early helps catch problems before calibration or imaging.

---

## Visualising Data with `plotms`

The `plotms` task is used to visualise visibility data and calibration tables.
It provides a graphical way to inspect amplitudes, phases, and other quantities.

A simple example to check that plotting works is:

```bash
plotms(
    vis='example.ms',
    xaxis='uvdist',
    yaxis='amp',
    avgchannel='128'
)
```

This opens the `plotms` window and displays amplitude as a function of
UV-distance.

At this stage, the goal is **not detailed analysis**, but to confirm that:

* The Measurement Set can be read
* Data are present and non-zero
* The plotting interface launches correctly

---

## Plotting Calibration Tables

After running a calibration script, CASA produces calibration tables that
can also be inspected using `plotms`.

For example:

```bash
plotms(
    vis='example_calibration.cal',
    xaxis='time',
    yaxis='phase',
    antenna='1,2,3,4,5',
    iteraxis='antenna'
)
```

This allows inspection of phase behaviour over time for different antennas.

## Why This Step Is Important

Using `listobs` and `plotms` early in the workflow:

* Confirms that data ingestion was successful
* Provides an initial quality check on the dataset
* Helps identify issues before calibration and imaging
* Builds intuition for how interferometric data behave

---
## What Can Go Wrong When Using `listobs` and `plotms`

While `listobs` and `plotms` are robust CASA tasks, several common issues can
prevent them from running as expected. Identifying these early can save
significant debugging time later in the workflow.

### Measurement Set Not Found

If the Measurement Set name or path is incorrect, CASA will fail to locate
the dataset.

Example error:
- File not found
- MS does not exist

To diagnose this:
```bash
pwd
ls

````

Ensure that the `.ms` directory exists in the current working directory and
that the name matches exactly.

---

### Running CASA from the Wrong Directory

CASA tasks assume relative paths unless absolute paths are provided. Running
CASA from an unintended directory can result in CASA being unable to locate
the Measurement Set.

Best practice:

* Always launch CASA **from within the working directory**
* Confirm location using:

```bash
pwd
```

---

### Plot Window Does Not Appear

In some cases, `plotms` may execute but no graphical window appears.

Possible causes include:

* Running CASA on a remote system without X-forwarding
* GUI backend not configured correctly
* Plot window opening behind other windows

Check the CASA logger and terminal for warnings or error messages.

---

### Empty or Flat Plots

If `plotms` produces a plot with no visible structure, this may indicate:

* Incorrect axis selections
* Over-averaging (e.g. too large `avgchannel`)
* Data flagged or missing

Try simplifying the plot:

```bash
plotms(vis='example.ms')
```

Then gradually add parameters once data visibility is confirmed.

---

### Incorrect Field or Antenna Selection

Specifying a field or antenna that does not exist in the Measurement Set will
result in empty plots or errors.

Use `listobs` to confirm valid field names, IDs, and antenna numbers before
plotting.

---

### CASA Logger vs Terminal Output

Some messages appear only in the CASA logger and not in the terminal.
If a task appears to fail silently, always check the logger for additional
information.

---

### Long Wait Times When Using `plotms`

In some cases, running `plotms` with many parameters or on large datasets can
result in long execution times or an unresponsive plotting window.

To avoid this, CASA provides an interactive workflow using `tget` that allows
parameters from the **previous `plotms` run** to be reused and adjusted
incrementally.

---

### Using `tget plotms` for Faster Iteration

After running `plotms` at least once, retrieve the last-used parameters with:

```bash
tget plotms
```

This loads the previous plotms settings into the CASA task interface.

From here, parameters can be adjusted selectively (for example, reducing the
number of channels or antennas) without redefining the entire command.

Once satisfied, press `go` in the plotms GUI to generate the plot.
