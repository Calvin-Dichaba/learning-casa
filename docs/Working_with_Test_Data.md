---
This is a recommended workflow for handling test datasets
when starting CASA, with a focus on directory organisation and common
terminal related pitfalls.

## Why Directory Structure Matters

When working with CASA, it is best practice **not to run CASA in the same
directory where raw or compressed data are stored**. Mixing data, scripts,
and CASA-generated outputs can lead to confusion and, in some cases, errors.

Separating data preparation from CASA execution helps keep workflows clean
and reproducible.

---

## Creating a Dedicated Working Directory

First, create a directory that will be used specifically for CASA work:

```bash
mkdir test_data_casa
````
## Navigating to the Data Location

Before copying any files, it is important to confirm where you are currently
located in the filesystem. This helps avoid copying data into the wrong
directory or running CASA from an unintended location.

To check your current directory, use:

```bash
pwd

````

If the compressed test dataset (`test_data.tar.gz`) is located elsewhere,
navigate to that directory using `cd`:

```bash
cd /path/to/data/location
```

After changing directories, always verify that you are in the correct place
and that the dataset is present:

```bash
pwd
ls
```

You should see `test_data.tar.gz` listed in the output. If the file is not
visible, double-check the path or directory name before proceeding.

---

## Copying the Dataset into the CASA Working Directory

Once you are in the directory containing `test_data.tar.gz`, copy it into the
dedicated CASA working directory (`test_data_casa`):

```bash
cp test_data.tar.gz /path/to/test_data_casa/
```

Then move into the CASA working directory:

```bash
cd /path/to/test_data_casa
```

Confirm that the file has been copied successfully:

```bash
ls
```

Finally, extract the contents of the archive:

```bash
tar -zxvf test_data.tar.gz
```

This ensures that CASA operates only on files within the dedicated working
directory, reducing the risk of path-related errors.

````
## Common Pitfall: Trailing Spaces in Directory Names

A subtle error encountered during this workflow was the presence of a
**trailing space in the name of a directory created to store test data**.

For example, a directory was created as:

```bash
DARA␣
````

Although this appears identical to `DARA`, the trailing space means the
terminal treats it as a *different directory*. This resulted in errors when
attempting to change into the directory or copy files into it.

Commands such as:

```bash
cd home/calvindichaba/documents/DARA
cp test_data.tar.gz home/calvindichaba/documents/DARA/
```

failed because the directory name did not exactly match what was typed.

---

## Diagnosing the Problem

To identify hidden characters in directory names, use:

```bash
ls -b
```

This command reveals non-printing characters, making trailing spaces visible.

You may see output similar to:

```bash
DARA\ 
```

indicating that the directory name contains an unintended space.

---

## Fixing the Directory Name

Rename the directory to remove the trailing space:

```bash
mv "DARA " DARA
```

After renaming, confirm the correction:

```bash
ls
pwd
```

You should now be able to navigate into the directory normally:

```bash
cd DARA
```

## Lesson Learned

Trailing spaces in directory names are difficult to spot but can cause
confusing path and file not found errors. Verifying directory names early
using `ls -b` can save significant debugging time when working with CASA
and the Linux terminal.
