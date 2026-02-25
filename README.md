# Testing Metadata Import into cBioPortal (Local Environment)

These instructions describe how to test the import of your study metadata into a **local cBioPortal instance** before
submitting it for inclusion in the official EGA cBioPortal.

We strongly recommend validating your metadata locally to ensure that formatting and structure are correct before
submission.

---

## 1. Study Metadata Format

To prepare your study metadata, please follow the official cBioPortal documentation:

- **File format specification**  
  https://docs.cbioportal.org/file-formats/

- **General cBioPortal documentation**  
  https://docs.cbioportal.org/

> [!IMPORTANT]  
> Your study directory must include all required `meta_*.txt` and `data_*.txt` files as described in the documentation.

---

## 2. Set Up a Local cBioPortal Instance

### 2.1 Clone the cBioPortal Docker Repository

```bash
git clone https://github.com/cBioPortal/cbioportal-docker-compose.git
cd cbioportal-docker-compose
git checkout 51468668cc49525b5f665c552608d50232b7e336
```

> [!NOTE]  
> Make sure you check out the specified commit to ensure compatibility with our production environment.

---

### 2.2 Download Required Seed Data

Run the initialization script:

```bash
./init.sh
```

This downloads seed database data, example configuration and example study data (from the cBioPortal datahub) needed.


---

### 2.3 Start Docker Containers

Start the application:

```bash
docker compose up -d
```

> [!CAUTION]
> Do not interrupt the database initialization process.
> It can take some minutes to import all the database data the first time.


Once running, cBioPortal should be available at `http://localhost:8080`

You can check container status with:

```bash
docker compose ps
```

---

## 3. Load Your Study Metadata

Put your study data in a new directory, i.e `my_cbio_study_data`, inside the `./studies/` directory.

Your study must follow the cBioPortal directory structure.

### 3.1 Import the Study

Run the following command:

```bash
docker compose exec cbioportal metaImport.py \
  -u http://cbioportal:8080 \
  -s studies/my_cbio_study_data/ \
  -o
```

If the import completes successfully, you should not see errors on the console and see an update to the study status
to 'AVAILABLE'.

---

## 4. Verify the Import

Please check the import console to ensure the there were no errors at any point of the process and visit your local
cBioPortal installation and verify: 

   - Your study appears in the study list
   - The study name and description are correct
   - Clinical attributes are visible
   - No data formatting errors are shown
   - All expected samples and patients are present
