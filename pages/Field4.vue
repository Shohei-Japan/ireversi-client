<template>
    <div class="board">
        <div
            class="cell"
            v-for="cell in cells"
            :key="cell.id"
            @click="putDisc(cell.id)"
        >
            <div
                v-if="cell.exist"
                :class="['disc', cell.isBlack ==1 ? 'black' : 'white']"
            />
        </div>
    </div>

</template>

<script>
export default {
    data() {
        const cells = [];
        for(let i = 0; i < 64; i++) {
            cells.push({ 
                id: i, 
                exist: null, 
                isBlack: 0
            });
        }
        console.log(cells);
        cells[27]={id: 27, exist:true, isBlack: -1},
        cells[28]={id: 28, exist:true, isBlack: 1},
        cells[35]={id: 35, exist:true, isBlack: 1},
        cells[36]={id: 36, exist:true, isBlack: -1}
        return {
            isBlackTurn: 1,
            cells,
        }
        // var fliped = [1,2]
        let dirs = [-9, -8, -7, -1, 1, 7, 8, 9]
    },
    methods: {
        putDisc(id){
            console.log(this.cells[id].exist)
            console.log('クリックした')

            this.flipDisc(this.cells[id].id)
            console.log('flipDisc処理終えた')

            this.cells = this.cells.map(a => a.id !== id ? a : {
                ...a,
                exist: true,
                isBlack: this.isBlackTurn,
            });

            this.isBlackTurn *= -1; //ターンチェンジ
            console.log('this.isBlackTurnは'+ this.isBlackTurn)
        },
        flipDisc(id) {
            let result = []
            for(let i = 0; i < 8; i++){
                let flipped = this.getFlipCellsOneDir(id, this.dirs[i], this.isBlackTurn)
                result = Object.assign(result,flipped);
                console.log('resultは ' + result)
            }
            console.log('★resultは ' + result)
            return result;
        },
        getFlipCellsOneDir(id, dir, isBlackTurn) {
            let dirId = id + dir
            console.log('idは'+id)
            // console.log('dirは'+dir)
            console.log('dirIdは'+dirId)
            console.log('this.cells[dirId].isBlackは' + this.cells[dirId].isBlack)
            console.log('isBlackTurnは'+ isBlackTurn)
            console.log('this.cells[dirId].existは ' + this.cells[dirId].exist)
            if(dirId % 8 === 0 || dirId % 8 === 7 || dirId < 8 || dirId >= 56 || this.cells[dirId].isBlack === isBlackTurn || !this.cells[dirId].exist){
                return false
            }
            var fliped = []
            fliped.push(dirId);
            console.log('flipedは'+ fliped)

            while (true) {
                if(dirId % 8 === 0 || dirId % 8 === 7 || dirId < 8 || dirId >= 56 || !this.cells[dirId].exist) {
                    console.log('●')
                    return false;
                }
                if (this.cells[dirId].isBlack*-1 === isBlackTurn){
                    console.log('●●')
                    return fliped;
                } else {
                    console.log('●●●')
                    fliped.push(dirId)
                }
            }
            console.log('getFlipCellsOneDirの処理を終えた')
        }
    }
}

</script>

<style>
    .board {
        width: 400px;
        height: 400px;
        font-size: 0;
    }
    .cell {
        margin: 0;
        padding: 0;
        background-color: #00B600;
        border: 1px solid #333;
        box-sizing: border-box;
        display: inline-block;
        width: 50px;
        height: 50px;
    }
    .disc {
        height: 50px;
        width: 50px;
        border-radius: 50%;
    }

    .black {
        background: #000;
    }

    .white {
        background: #fff;
    }
</style>
