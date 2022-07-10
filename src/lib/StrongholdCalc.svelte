<script>
    import { calcIntersectionPoint, solve, averagePoint } from "@liuxsdev/utils";
    import errorUrl from "../assets/error.svg";
    let points = [{ x: "", z: "", degree: "" }];
    $: points_numbered = points_to_number(points);
    $: stronghold = calc(points_numbered);
    function addPoint() {
        points = [...points, { x: "", z: "", degree: "" }];
    }
    function points_to_number(point) {
        return point.map((item) => {
            return {
                x: parseFloat(item.x),
                z: parseFloat(item.z),
                degree: parseFloat(item.degree),
            };
        });
    }

    function check_points(points) {
        if (points.length == 1 && points[0].x) {
            throw " need more points";
        }
        for (let i = 0; i < points.length; i++) {
            if (!points[i].x || !points[i].z || !points[i].degree) {
                throw " x or z or degree is Empty";
            }
        }
    }

    function calc(points) {
        try {
            check_points(points);
            let inters = solve(points);
            let result = averagePoint(inters);
            return result;
        } catch (err) {
            return err;
        }
    }
    function clearPoint() {
        points = [{ x: "", z: "", degree: "" }];
    }
</script>

<div class="point-container">
    <div id="pointarea">
        {#each points as point}
            <div class="item">
                <div class="points">
                    <span>x:</span>
                    <input type="text" class="point-input" bind:value={point.x} />
                </div>
                <div class="points">
                    <span>z:</span>
                    <input type="text" class="point-input" bind:value={point.z} />
                </div>
                <div class="points">
                    <span>θ:</span>
                    <input type="text" class="point-input" bind:value={point.degree} />
                </div>
            </div>
        {/each}
    </div>

    <div class="control">
        <button on:click={clearPoint}>clear</button>
        <button on:click={addPoint}>add points</button>
        <!-- <button on:click={calc}>calc</button> -->
    </div>
    <div class="result">
        {#if typeof stronghold == "object"}
            <div class="alert-body">
                <div class="alert-title">Result:</div>
                <div class="alert-body-content">
                    <span>x坐标:{stronghold.x.toFixed(2)}</span>
                    <span>z坐标:{stronghold.z.toFixed(2)}</span>
                </div>
            </div>
        {:else}
            <div class="result-error">
                <div class="alert-icon">
                    <img src={errorUrl} alt="" srcset="" />
                </div>
                <div class="alert-body">
                    <div class="alert-title">Error:</div>
                    <div class="alert-body-content">{stronghold}</div>
                </div>
            </div>
        {/if}
    </div>
</div>

<style>
    .points {
        padding: 0.5em;
    }
    .point-container {
        align-self: center;
    }

    input.point-input {
        width: 100%;
    }
    .item {
        display: flex;
        flex-direction: row;
        justify-content: center;
    }
    .control {
        text-align: right;
        margin-top: 10px;
    }
    #pointarea {
        border: 1px solid #707070;
        padding: 15px;
        border-radius: 10px;
    }
    button {
        height: 24px;
    }
    .alert-icon {
        width: 1em;
        margin-right: 1em;
    }
    img {
        width: 100%;
    }
    .result-error {
        display: flex;
    }
    .result {
        background-color: #fbeef1;
        height: 4em;
        padding: 1em;
        margin-top: 1em;
    }
    .alert-body-content {
        margin-top: 0.5em;
    }
</style>
