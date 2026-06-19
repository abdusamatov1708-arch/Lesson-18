# Lesson-18
import json
from dataclasses import dataclass, field, asdict
from datetime import datetime
from typing import Callable, Any


@dataclass(order=True)
class Vazifa:
    sana: datetime = field(compare=True)
    id: int = field(compare=False)
    matn: str = field(compare=False)
    bajarildi: bool = field(compare=False)
    teglar: list[str] = field(default_factory=list, compare=False)


@dataclass(frozen=True)
class VazifaSnapshot:
    id: int
    matn: str
    bajarildi: bool
    sana: datetime
    teglar: tuple[str, ...]


def filtrlash(vazifalar: list[Vazifa], shart: Callable[[Vazifa], bool]) -> list[Vazifa]:
    return [v for v in vazifalar if shart(v)]


def snapshot_olish(vazifa: Vazifa) -> VazifaSnapshot:
    return VazifaSnapshot(
        id=vazifa.id,
        matn=vazifa.matn,
        bajarildi=vazifa.bajarildi,
        sana=vazifa.sana,
        teglar=tuple(vazifa.teglar)
    )


def asosiy():
    vazifalar = [
        Vazifa(id=1, matn="Python o'rganish", bajarildi=False, sana=datetime(2026, 6, 20), teglar=["python", "dars"]),
        Vazifa(id=2, matn="Uy ishlari", bajarildi=True, sana=datetime(2026, 6, 18), teglar=["uy"]),
        Vazifa(id=3, matn="Do'kon borish", bajarildi=False, sana=datetime(2026, 6, 19), teglar=["xarid"])
    ]

    bajarilmaganlar = filtrlash(vazifalar, lambda v: not v.bajarildi)
    print("Bajarilmagan vazifalar:", [v.matn for v in bajarilmaganlar])

    snapshot = snapshot_olish(vazifalar[0])
    print("\nSnapshot yaratildi:", snapshot)
    json_malumot = json.dumps(
        [asdict(v) for v in vazifalar],
        default=str,
        ensure_ascii=False,
        indent=4
    )
    print("\nJSON formatdagi ma'lumotlar:")
    print(json_malumot)


if __name__ == "__main__":
    asosiy()
