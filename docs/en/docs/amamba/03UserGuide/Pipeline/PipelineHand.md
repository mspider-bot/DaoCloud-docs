# Manually execute the pipeline

This page describes manual pipeline execution on the GUI, including immediate execution, rerun, and cancel run.

## Execute the pipeline immediately

1. Select a pipeline on the pipeline list page, click `︙`, and click `Run Now` in the pop-up menu.

   ![handrun01](../../images/handrun01.jpeg)

2. Depending on whether the pipeline is configured with `build parameters`, the following two situations will occur after execution:

   - If `Build Parameters` is configured, a dialog box will appear and display relevant content for parameter configuration.

     ![handrun02](../../images/handrun02.jpeg)

   - If no `build parameter` is configured, the pipeline executes immediately.

3. The pipeline starts executing.

   ![handrun03](../../images/handrun03.jpeg)

## Rerun the pipeline

On the pipeline details page, according to the execution ID in the pipeline running record, you can `rerun` the pipeline that has already been run.

1. In the pipeline list, click the name of a certain pipeline to enter the pipeline details page.

2. In the `Run Record` area, find the `Execution ID` that needs to be re-run.

3. Click `︙` on the right, and click `Rerun` in the pop-up menu.

   ![handrun04](../../images/handrun04.jpeg)

4. The pipeline reruns successfully.

   ![handrun05](../../images/handrun05.jpeg)

## Cancel running the pipeline

On the pipeline details page, according to the `execution ID` in the pipeline running record, you can `cancel` the executing pipeline.

1. In the pipeline list, click the name of a certain pipeline to enter the pipeline details page.

2. In the `Operation Record` area, find the `Execution ID` that needs to be canceled.

3. Click `︙` on the right, and click `Cancel` in the pop-up menu (applicable to pipelines whose status is `Executing`).

   ![handrun06](../../images/handrun06.jpeg)

4. The pipeline cancellation operation is successful.

   ![handrun07](../../images/handrun07.jpeg)